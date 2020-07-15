---
title:  "Collaboration : Remote Runs - Terraform Features"
search: true
toc: true
toc_sticky: true
comments: true
categories: 
  - HashiCorp
  - Terraform
---

Terraform의 Remote Runs이라는 기능에 대해 확인합니다.

Terraform Cloud와 Terraform Enterprise는 원격으로 트리거링 되어 동작하는 메커니즘을 제공하고 있습니다.



[ Terraform | Features ]

# Collaboration : Remote Runs

<iframe width="560" height="315" src="https://www.youtube.com/embed/pSPZQdpWWjs" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Terraform Cloud와 Terraform Enterprise에서는 수행의 주체가 중앙 서버이며, 등록된 VCS나 Terraform  설정 파일의 구성을 통해 원격으로 트리거링할 수 있습니다.

---

| 유형                                 | 지원여부 |
| ------------------------------------ | -------- |
| Terraform OSS (Open Source Software) | •        |
| Terraform Cloud                      | ✔︎        |
| Terraform Cloud for Business         | ✔︎        |
| Terraform Enterprise                 | ✔︎        |

---

원격으로 실행시키는 메커니즘은 총 3가지 형태가 있습니다.

- VCS에서 Pull 요청시
- CI/CD 과정에서 API를 통한 호출
- CLI로 수행



## VCS Pull Request

워크스페이스는 VCS와 연동하는 것이 일반적이며, 이 경우 VCS에 Pull이 발생하면 이를 감지하여 해당 워크스페이스의 Run이 수행되는 케이스 입니다.

![Version Control | Version Control | great-stone | Terraform Cloud 2020-07-15 23-01-23](https://raw.githubusercontent.com/Great-Stone/images/master/uPic/Version%20Control%20|%20Version%20Control%20|%20great-stone%20|%20Terraform%20Cloud%202020-07-15%2023-01-23.png)

VCS에 Pull이 발생하는 것은 최종적으로 검증된 코드가 올라왔다고 판단되어 동작하며, 특정 파일이나 경로에 관련 동작이 발생했을 경우에만 Run이 수행되도록 설정 가능합니다.



## API call

별도의 CI/CD 파이프라인과 연계하여 사용하는 경우에는 API를 호출하여 실행시키는 방식을 제공합니다. 인프라의 변경 뿐만 아니라 애플리케이션과 상호 작용하는 경우에 유용하며, 인가된 사용자를 구분하기 위해 Token을 필요로 합니다. 주의해야 할 점은 문서상에 나와있는 것 처럼 Organization Token이 아닌 User나 Team의 Token으로 요청해야 합니다. [문서 링크](https://www.terraform.io/docs/cloud/api/run.html#create-a-run)

처음 워크스페이스에 Run을 요청하는 JSON 형태의 data의 예는 다음과 같습니다. 

<script src="https://gist.github.com/Great-Stone/02a146bfc937fc116a8a7a9a067550a7.js"></script>

작성된 json파일을 POST 로 요청하는 형태는 다음과 같습니다.

```bash
curl \
  --header "Authorization: Bearer $TF_CLOUD_TOKEN" \
  --header "Content-Type: application/vnd.api+json" \
  --request POST \
  --data @run.json \
  https://app.terraform.io/api/v2/runs
```



Run을 요청한 응답 데이터에는 관련 `ìd`와 유형, 기타 정보들이 담겨있습니다.

```json
{
  "data": {
    "id": "run-CZcmD7eagjhyX0vN",
    "type": "runs",
    "attributes": {
      "auto-apply": false,
      "error-text": null,
      ...
```

Terraform 웹 콘솔에 접속하여 확인해보면 해당 `ìd`값을 갖는 Run이 수행됨을 확인 할 수 있습니다.

이후 apply를 위해 `comment`가 담긴 데이터를 생성하고, 앞서 응답받은 Run의 `id` 경로로 요청을 보냅니다.

<script src="https://gist.github.com/Great-Stone/3105a9c66e6754352c5b2dd26e31fbfa.js"></script>

```bash
curl \
  --header "Authorization: Bearer $TF_CLOUD_TOKEN" \
  --header "Content-Type: application/vnd.api+json" \
  --request POST \
  --data @apply.json \
  https://app.terraform.io/api/v2/runs/run-CZcmD7eagjhyX0vN/actions/apply
```

API 호출을 사용하면 다양한 시스템을 활용하여 Terraform을 수행할 수 있습니다.



## CLI 수행

Terraform을 OSS로 사용하시는 분들은 커맨드 항목 중에 `login`, `logout`이 있는 것을 보셨을 수 있습니다. 해당 커맨드는 Terraform Cloud나 Terraform Enterprise를 활용하여 프로비저닝을 수행할 수 있는 커맨드 입니다. 원격으로 실행시키기 위해서는 우선 대상에 대한 정의가 필요합니다.

```javascript
terraform {
  backend "remote" {
    organization = "great-stone"
    workspaces {
      name = "random-pet-demo"
    }
  }
}
```

tf 파일에 `backend`에 대한 설정을 추가하면 테라폼 실행시 원격의 서버와 통신하여 작업을 수행합니다. 워크스페이스의 경우 별도 VCS를 연동하지 않고 로컬과 연계하여 동작하게 됩니다. 또한 Apply 시 `yes`에 대한 동작도 웹콘솔에서 수행던 로컬에서 수행하던 서로 동기화 되어 실행 됩니다.



## 마치며

VCS, API, CLI 모두 Terraform의 Run을 수행하는 동일한 동작을 수행합니다. 각 팀, 조직에서의 프로비저닝을 위한 관리 방식에 따라 가장 알맞은 방식으로 접근할 수 있는 메커니즘을 잘 활용하면 효율적인 자원 관리가 가능합니다.







서 설명드렸듯 HCL은 JSON과 호환되고 이런 방식이 더 자연스러우신 분들은 JSON으로 관리가 가능합니다. 하지만 HCL에 대한 일반적인 질문은 왜 JSON이나 YAML같은 방식이 아닌지 입니다.

HCL 이전에 HashiCorp에서 사용한 도구는 Ruby같은 여타 프로그래밍 언어를 사용하여 JSON같은 구조를 만들어내기 위해 다양한 언어를 사용했습니다. 이때 알게된 점은 어떤 사용자는 인간 친화적인 언어를 원했고 어떤 사람들은 기계 친화적 언어를 원한다는 것입니다.

JSON은 이같은 요건 양쪽에 잘 맞지만 상당히 구문이 길어지고 주석이 지원되지 않는 다는 단점이 있습니다. YAML을 사용하면 처음 접하시는 분들은 실제 구조를 만들어내고 익숙해지는데 어려움이 발생하였습니다. 더군다나 일반적 프로그래밍 언어는 허용하지 않아야하는 많은 기능을 내장하고 있는 문제점도 있었습니다.

이런 여러 이유들로 JSON과 호환되는 자체 구성언어를 만들게 되었고 HCL은 사람이 쉽게 작성하고 수정할 수 있도록 설계되었습니다. 덩달아 HCL용 API가 JSON을 함께 호환하기 때문에 기계 친화적이기도 합니다.

![Terraform_Features_Master(KR) - Google Slides 2020-07-14 22-28-30](https://raw.githubusercontent.com/Great-Stone/images/master/uPic/Terraform_Features_Master%28KR%29%20-%20Google%20Slides%202020-07-14%2022-28-30.png)

HCL을 익히고 사용하는 건 어떨까요?

예를 들어 Python 코드로 비슷하게 정의를 내려보았습니다. 우리가 사용할 패키지를 Import하고 해당 패키지가 기본적으로 필요로하는 값을 넣어 초기화 합니다. 여기서는 `aws`라는 패키지에 `region`과 `profile` 이름을 넣어서 기본적으로 동작 할 수 있는 설정으로 초기화 하였습니다. 이후에 해당 패키지가 동작할 수 있는 여러 서브 펑션들에 대한 정의를 하고 마지막으로는 실행을 위한 큐에 넣습니다.

HCL도 거의 이런 일반적 프로그래밍의 논리와 비슷합니다. 우선 사용할 **프로바이더** 라고하는 마치 라이브러리나 패키지 같은 것을 정의 합니다. 이 프로바이더에는 기본적으로 선언해주어야 하는 값들이 있습니다.

그리고 이 프로바이더가 제공하는 기본적인 모듈들, 즉, 클래스나 구조체와 비슷한 형태로 정의 합니다. `resource`에 대한 정의는 마치 `aws_instance`라는 클래스를 `example`로 정의하는 것과 비슷한 메커니즘을 갖습니다. 그리고 해당 리소스의 값들을 사용자가 재정의 하는 방식입니다.



## HCL Syntax

실제 HCL의 몇가지 예는 다음과 같습니다. ([github](https://github.com/hashicorp/hcl))

```python
// 한줄 주석 방법1
# 한줄 주석 방법2

/*
다중
라인
주석
*/

locals {
  key1 =      "value1" // = 를 기준으로 키와 값이 구분되며
  key2     = "value2"  // = 양쪽의 공백은 중하지 않습니다.
  myStr = "TF ♡ UTF-8" // UTF-8 문자를 지원합니다.
  multiStr = <<FOO     // <<EOF 같은 여러줄의 스트링을 지원합니다.
  Multi
  Line
  String
  with <<ANYTEXT
  FOO                  // 앞과 끝 문자만 같으면 됩니다.
  
  boolean1 = true      // boolean true
  boolean2 = false     // boolean false를 지원합니다.

  deciaml = 123        // 기본적으로 숫자는 10진수,
  octal = 0123         // 0으로 시작하는 숫자는 8진수,
  hexadecimal = "0xD5" // 0x 값을 포함하는 스트링은 16진수,
  scientific = 1e10    // 과학표기 법도 지원합니다.

  //funtion 들이 많이 준비되어있습니다.
  myprojectname = format("%s is myproject name", var.project)
  //인라인 조건문도 지원합니다.
  credentials = var.credentials == "" ? file(var.credentials_file) : var.credentials
}
```

[Funtion Doc](https://www.terraform.io/docs/configuration/functions.html)

---

Terraform의 가장 기본적인 Infrastructure as Code에 대한 소개와 이를 구현하는 HCL에 대해 알아보았습니다.

