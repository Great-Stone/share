---
title:  ".Net(dotNet) core & OpenShift"
search: true
toc: false
toc_sticky: false
comments: true
categories: 
  - OpenShift
---
.NET Framework는 Windows 장치에서만 작동했습니다. Xamarin 및 Mono 프로젝트는 .NET을 모바일 장치, macOS 및 Linux에 제공하기 위해 시도했고, .NET Core에서 Windows, Linux, MacOS 및 모바일 장치에서 Xamarin을 통해 사용할 수있는 표준 기본 라이브러리를 제공합니다.



## 1. .Net 아키텍처

.NET 아키텍처에는 네 가지 주요 구성 요소가 있습니다.

- CLS ( [Common language specification](https://docs.microsoft.com/en-us/dotnet/standard/common-type-system) )는 .NET이 작동하는 모든 곳에서 동작하도록 객체가 구현되는 방식을 정의합니다. CLS는 모든 유형을 설명하는 공통적 인 방법을 설정하는 CTS (Common Type System)의 하위 집합입니다.
- [프레임 워크 클래스 라이브러리](https://msdn.microsoft.com/en-us/library/gg145045(v=vs.110).aspx) (FCL)는 재사용 가능한 클래스, 인터페이스 및 값 유형을 수집하는 표준 라이브러리입니다.
- CLR ( [Common Language Runtime](https://docs.microsoft.com/en-us/dotnet/standard/clr) )은 프레임 워크를 실행하고 .NET 프로그램의 실행을 관리하는 가상 컴퓨터입니다.
- [Visual Studio](https://www.visualstudio.com/) 와 같은 도구로 독립 실행 형 응용 프로그램, 대화 형 웹 사이트, 웹 응용 프로그램 및 웹 서비스를 만들 수 있습니다.

## 2. .Net core 특징

### 1.1 .Net core와 .Net framwork의 차이

두 구현 방법은 여러 가지 동일한 구성 요소를 공유하므로 둘 간에 코드를 공유할 수 있습니다. 그러나 이들 간에는 기본적인 차이가 있으며 수행할 항목에 따라 선택이 달라집니다. Microsoft는 .NET으로 응용 프로그램을 빌드 할 때 두 가지 런타임을 모두 유지하며 동일한 API를 많이 공유합니다. 이 공유 API는 .NET 표준이라고합니다.

![.net core vs .net frameworkì ëí ì´ë¯¸ì§ ê²ìê²°ê³¼](C:\Users\gyulee\Dropbox\Markup\images\DotNet-Architecture.jpg)



개발자는 .NET Framework를 사용하여 Windows 데스크톱 응용 프로그램과 서버 기반 응용 프로그램을 만듭니다. 여기에는 ASP.NET 웹 응용 프로그램이 포함됩니다. .NET Core는 Windows, Linux 및 Mac에서 실행되는 서버 응용 프로그램을 만드는 데 사용됩니다. 현재는 사용자 인터페이스가있는 데스크톱 응용 프로그램을 만들 수 없습니다. 개발자는 두 런타임 모두에서 VB.NET, C # 및 F #으로 응용 프로그램과 라이브러리를 작성할 수 있습니다.

[C #은](https://docs.microsoft.com/en-us/dotnet/articles/csharp/index) 다른 C 스타일의 언어와 비슷한 [객체 지향](https://docs.microsoft.com/en-us/dotnet/articles/csharp/index) 언어입니다. 학습 곡선은 이미 C 및 유사한 언어로 작업하는 개발자에게는 문제가되지 않습니다.

[F #은](https://docs.microsoft.com/en-us/dotnet/articles/fsharp/index) 객체 지향 프로그래밍을 사용 [하는 크로스 플랫폼 언어](https://docs.microsoft.com/en-us/dotnet/articles/fsharp/index) 입니다.

Visual Basic은 [.NET Core 2.0](https://stackify.com/net-core-2-0-changes/) 에서 제한된 .NET Core를 지원하는 [.NET](https://stackify.com/net-core-2-0-changes/)Framework에서 사용할 수 있습니다 .



**다음과 같은 경우에는 서버 응용 프로그램에 .NET Core를 사용합니다.**

- 교차 플랫폼 요구 사항이 있습니다. 

  응용 프로그램을 Windows, Linux 및 macOS와 같은 여러 플랫폼에서 실행해야하는 경우에 사용하십시오. 이러한 운영 체제는 개발 워크 스테이션으로 지원되며 

  지원되는 운영 체제

  목록 이 늘어납니다.

  - Visual Studio는 Windows [에서 MacOS](https://www.visualstudio.com/vs/visual-studio-mac/) 의 새로운 제한된 버전으로 사용할 수 있습니다 .
  - Visual Studio 코드는 Windows, Linux 및 MacOS에서 사용할 수 있습니다.
  - 명령 줄은 지원되는 모든 플랫폼에서 사용할 수 있습니다.

- **마이크로 서비스가 사용되고 있습니다.** 서비스 중심 아키텍처의 한 형태 인 [Microservices](https://stackify.com/what-are-microservices/) 는 소규모의 모듈 식 비즈니스 서비스로 구성된 소프트웨어 응용 프로그램입니다. 각 서비스는 고유 한 프로세스를 실행할 수 있으며, 독립적으로 배포되고 다른 프로그래밍 응용 프로그램에서 생성 될 수 있습니다. .NET Core는 다양한 기술을 사용할 수 있으며 가볍고 각 마이크로 서비스에 대해 최소화 할 수 있습니다. 새로운 마이크로 서비스가 추가됨에 따라 확장 가능합니다.

- **Docker 컨테이너를 사용할 때.** 컨테이너와 마이크로 서비스 아키텍처는 종종 함께 사용됩니다. 가볍고 모듈 형이기 때문에 .NET Core는 컨테이너와 잘 작동합니다. 서버 앱은 Docker 컨테이너에 플랫폼 간 배포 할 수 있습니다. .NET Framework는 컨테이너에 사용할 수 있지만 이미지 크기가 더 큽니다.

- **고성능 및 확장 가능한 시스템 요구 사항이있는 경우** 최상의 성능과 확장을 위해 .NET Core with ASP.NET Core를 실행하는 것이 좋습니다. 수백 가지의 마이크로 서비스를 사용할 수있게되면 이것이 중요해진다. 더 적은 수의 서버 및 가상 시스템이 필요합니다. 얻은 효율성과 확장 성은 비용 절감 외에도 향상된 사용자 경험으로 이어질 수 있습니다.

- **여러 .NET 버전을 나란히 실행하는 경우** .NET Framework의 여러 버전에 종속 된 응용 프로그램을 설치하려면 개발자는 .NET Core를 사용해야합니다. 서로 다른 버전의 .NET을 사용하는 동일한 서버에서 여러 서비스를 실행할 수 있습니다.

- **명령 줄 인터페이스 (CLI) 제어를 원할 경우** 일부 개발자는 경량 편집기 및 명령 줄 컨트롤 작업을 선호합니다. .NET Core에는 지원되는 모든 플랫폼에 대한 CLI가 있습니다. 그것은 생산 기계에 최소한의 설치가 필요합니다. 그리고 여전히 Visual Studio IDE와 같은 IDE로 전환 할 수있는 기회가 있습니다.



**다음과 같은 경우에는 서버 응용 프로그램에 .NET Core 대신 .Net Framwork를 사용합니다.**

- **Windows Forms 및 WPF 응용 프로그램** 은 지원되지 않습니다. MacOS 용 .NET 데스크탑 응용 프로그램을 만들기 위해서는 여전히 모노를 사용해야합니다.
- **ASP.NET WebForms은 존재하지 않습니다.** Microsoft는 ASP.NET 코어로 포팅 할 계획입니다.
- **WCF 서비스를 만들어야합니다.** .NET Core는 현재 WCF를 지원하지 않습니다. 대신 ASP.NET Core MVC를 사용하여 REST API를 만들어야합니다.
- **누락 된 타사 라이브러리 지원.** .NET Core 2.0은 .NET Framework와 .NET Core 간의 호환성을 제공합니다. 그러나 클래스 라이브러리가 지원되지 않는 .NET Framework API를 사용하는 경우에도 호환성 문제가있을 수 있습니다. 하지만이 방법은 .NET Core에 많은 클래스 라이브러리를 연결하는 데 도움이됩니다.
- **누락 된 .NET Framework 기능** 일부 .NET Framework 기능은 .NET Core에 아직 없습니다. 예를 들어, Entity Framework Core는 Entity Framework v6과 완전히 동일하지 않습니다.
- **Windows 관련 API에 액세스해야합니다.** 응용 프로그램이 Windows 레지스트리, WMI 또는 기타 Windows 관련 API를 사용해야하는 경우 .NET Core에서 작동하지 않습니다. OS에서 더 멀리 샌드 박싱되도록 설계되었습니다.
- **VB.NET 및 F #에 대한 부분 지원.** Microsoft와 커뮤니티는 계속이 작업을 수행하지만 아직 100 %는 아닙니다.
- **SignalR은 지원되지 않습니다.** .NET Core 2.1 용으로 계획되어 있으며 곧 제공 될 예정입니다.



### 1.2 .NET Core를 선택하는 경우



#### 멀티 플랫폼 배포 요건

응용 프로그램(웹/서비스)이 여러 플랫폼 (Windows, Linux 및 macOS)에서 실행되어야 하는 경우 .NET Core를 사용합니다.

.NET Core는 앞에서 언급한 운영 체제를 개발 워크스테이션으로 지원합니다. Visual Studio는 Windows 및 macOS용 IDE(통합 개발 환경)를 제공합니다. 또한 macOS, Linux 및 Windows에서 실행되는 Visual Studio Code를 사용할 수 있습니다. Visual Studio Code는 IntelliSense 및 디버깅을 포함하여 .NET Core를 지원합니다. Sublime, Emacs 및 VI 같은 대부분의 타사 편집기는 .NET Core에서 작동합니다. 이러한 타사 편집기는 Omnisharp를 사용하여 편집기 IntelliSense를 가져옵니다. 어떤 코드 편집기도 사용하지 않고, 지원되는 모든 플랫폼에서 사용할 수 있는 .NET Core CLI 도구를 직접 사용할 수도 있습니다.



#### 마이크로 서비스 아키텍처

.NET Framework, Java, Ruby 또는 다른 모놀리식 기술로 개발된 서비스나 마이크로 서비스를 융합할 수 있습니다.

참고 : [.NET 마이크로 서비스: 컨테이너화된 .NET 애플리케이션을 위한 아키텍처](https://docs.microsoft.com/ko-kr/dotnet/standard/microservices-architecture/index)



#### 고성능 및 확장 가능한 시스템에 대한 요구 사항

시스템에 가장 적합한 성능 및 확장성이 필요한 경우 .NET Core와 ASP.NET Core를 선택하는 것이 최고입니다.  



#### 컨테이너

컨테이너는 일반적으로 마이크로 서비스 아키텍처와 함께 사용됩니다. 또한 컨테이너는 모든 아키텍처 패턴을 따르는 웹앱 또는 서비스를 컨테이너화하는 데 사용할 수 있습니다. .NET Framework를 Windows 컨테이너에서 사용할 수 있지만 .NET Core의 모듈화된 간단한 특성이 컨테이너에 더 적합합니다. 컨테이너를 만들고 배포할 때 컨테이너 이미지의 크기가 .NET Framework보다 .NET Core를 사용할 때 훨씬 더 작습니다. 플랫폼 간 사용되므로 예를 들어 서버 앱을 Linux Docker 컨테이너에 배포할 수 있습니다.



## 3. .Net과 OpenShift

### 3.1 OpenShift에서의 .Net 지원 정보

이기종 데이터 센터. 기본 인프라가 Windows Server에만 의존하지 않고도 .NET 응용 프로그램을 실행할 수 있습니다. 

고객은 Red Hat Enterprise Linux 또는 Windows Server에서 배포 및 실행할 수 있습니다. 

.NET Core는 인증 컨테이너를 통해 Red Hat Enterprise Linux (RHEL 7) 및 OpenShift Container Platform에서 사용할 수 있습니다.

- 지원되는 버전
  - .NET Core 2.2
  - .NET Core 2.1
  - .NET Core 1.1
  - .NET Core 1.0
  - Red Hat Enterprise Linux (RHEL) 7 및 OpenShift 컨테이너 플랫폼 버전 3.3 이상에서 지원

RHEL 7 이미지는 Red Hat Registry를 통해 구할 수 있습니다.

```BASH
~$ docker pull registry.redhat.io/dotnet/dotnet-22-rhel7
~$ docker pull registry.redhat.io/dotnet/dotnet-21-rhel7
~$ docker pull registry.redhat.io/dotnet/dotnetcore-11-rhel7
~$ docker pull registry.redhat.io/dotnet/dotnetcore-10-rhel7
```



.Net S2I 빌드를 지원하며 환경변수로 다음의 항목을 지원합니다.([링크](https://docs.openshift.com/container-platform/3.11/using_images/s2i_images/dot_net_core.html#dot-net-core-configuration))

- DOTNET_STARTUP_PROJECT
- DOTNET_SDK_VERSION
- DOTNET_ASSEMBLY_NAME
- DOTNET_RESTORE_SOURCES
- DOTNET_TOOLS
- DOTNET_NPM_TOOLS
- DOTNET_TEST_PROJECTS
- DOTNET_CONFIGURATION
- DOTNET_VERBOSITY
- HTTP_PROXY, HTTPS_PROXY
- NPM_MIRROR
- ASPNETCORE_URLS
- DOTNET_RM_SRC
- DOTNET_SSL_DIRS
- DOTNET_RESTORE_DISABLE_PARALLEL



### 3.2 OpenShift에 .Net core 어플리케이션 배치 하기

관련 링크 : [CHAPTER 2. .NET CORE 2.0 ON RED HAT OPENSHIFT CONTAINER PLATFORM](https://access.redhat.com/documentation/en-us/net_core/2.0/html/getting_started_guide/gs_dotnet_on_openshift)

.Net core를 Openshift에 배치하기 위해 Image Stream를 생성합니다. [dotnet_imagestreams.json](https://raw.githubusercontent.com/redhat-developer/s2i-dotnetcore/master/dotnet_imagestreams.json) 파일에서는 .Net core 빌드를 위한 Base image를 정의하고 있습니다.

```bash
~$ oc create -f https://raw.githubusercontent.com/redhat-developer/s2i-dotnetcore/master/dotnet_imagestreams.json
```

Image Stream이 준비되면 git 링크를 통해 새로운 어플리케이션을 OpenShift에 빌드/배포할 수 있습니다. 다음 명령을 실행하여 GitHub 저장소 `app`의 `dotnetcore-2.0`브랜치의 `redhat-developer/s2i-dotnetcore-ex` ASP.NET 어플리케이션을 배포합니다.

```bash
~$ oc new-app --name=exampleapp 'dotnet:2.0~https://github.com/redhat-developer/s2i-dotnetcore-ex#dotnetcore-2.0' --build-env DOTNET_STARTUP_PROJECT=app
```



### 3.3 S2I를 사용하여 .NET 코어 컨테이너 이미지 작성

이미지를 빌드 할 수있는 세 가지 방법에 대해 설명합니다.  **생성되는 이미지를 OpenShift와 함께 사용하기 위해서는 빌드가 완료된 이미지를 <u>OpenShift Registry로 Push</u>해야하는 작업이 필요합니다.**

Fedora 및 [Red Hat Enterprise Linux](https://developers.redhat.com/products/rhel/overview/) (RHEL)에서 `source-to-image`패키지 ( `yum install source-to-image`)를 설치하여 S2I를 얻을 수 있습니다 . Windows 및 macOS를 포함한 다른 OS를 사용하는 경우 github.com/openshift/source-to-image/releases에서 [S2I](https://github.com/openshift/source-to-image/releases)를 다운로드 할 수 있습니다 .



#### A. git 저장소에서 애플리케이션 이미지 빌드하기

예제 `mywebapp`에서 [github.com/redhat-developer/s2i-dotnetcore-ex](https://github.com/redhat-developer/s2i-dotnetcore-ex) 에서 ASP.NET 핵심 응용 프로그램 용 응용 프로그램 이미지 를 작성 합니다.

```bash
~$ s2i build https://github.com/redhat-developer/s2i-dotnetcore-ex registry.redhat.io/dotnet/dotnet-21-rhel7:2.1 mywebapp -r dotnetcore-2.1 -e DOTNET_STARTUP_PROJECT=app -p always
```

이 명령을 실행 `s2i`하면 소스를 확인하고 .NET Core 응용 프로그램 이미지를 빌드합니다. 명령이 완료되면 새로 생성 된 이미지를 실행할 수 있습니다.

```bash
~$ docker run --rm -p 8080:8080 mywebapp
```

`s2i`명령에 전달한 매개 변수 정보는 다음과 같습니다.

| 매개 변수                              | 기술                                               |
| -------------------------------------- | -------------------------------------------------- |
| registry.redhat.io/dotnet-21-rhel7:2.1 | S2I 빌더 이미지                                    |
| https : // ... / s2i-dotnetcore-ex     | 자식 저장소                                        |
| -r dotnetcore-2.1                      | 저장소의 분기                                      |
| -e DOTNET_STARTUP_PROJECT = app        | 응용 프로그램 csproj 파일이 들어있는 저장소의 폴더 |
| -p always                              | 항상 최신 빌더 이미지 가져 오기                    |

`-e DOTNET_STARTUP_PROJECT`는 S2I 빌더에 전달하는 환경 변수입니다. 



#### B. 로컬 소스에서 응용 프로그램 이미지 작성

`s2i`명령 의 source 매개 변수 는 소스 코드가 들어있는 로컬 폴더를 참조 할 수도 있습니다. 다음 예제에서는 저장소를 로컬에서 복제하고 로컬 폴더에서 빌드합니다.

```bash
~$ git clone -b dotnetcore-2.1 https://github.com/redhat-developer/s2i-dotnetcore-ex
~$ s2i build s2i-dotnetcore-ex registry.redhat.io/dotnet/dotnet-21-rhel7:2.1 mywebapp -r dotnetcore-2.1 -e DOTNET_STARTUP_PROJECT=app -p always
```

이전 과 같은 `docker run` 명령을 사용하여 이미지를 실행할 수 있습니다 .



#### C. 미리 빌드 된 애플리케이션에서 애플리케이션 이미지 빌드하기

.NET Core 빌더를 사용하여 사전 빌드 된 응용 프로그램에서 응용 프로그램 이미지를 빌드 할 수 있습니다.

이를 탐색하기 위해 먼저 `s2i-dotnetcore-ex`응용 프로그램을 게시합니다 . 이를 위해서는 시스템에 .NET SDK ( [RHEL7](https://access.redhat.com/documentation/en-us/net_core/2.1/html/getting_started_guide/gs_install_dotnet#install_dotnet21) / [CentOS7](https://access.redhat.com/documentation/en-us/net_core/2.1/html/getting_started_guide/gs_install_dotnet#install_dotnet21) , [Fedora](https://fedoraloves.net/) , [기타](https://www.microsoft.com/net/download) ) 를 설치해야합니다 .

```bash
~$ git clone -b dotnetcore-2.1 https://github.com/redhat-developer/s2i-dotnetcore-ex
~$ cd s2i-dotnetcore-ex/app
~$ dotnet publish -c Release /p:MicrosoftNETPlatformLibrary=Microsoft.NETCore.App
```

`MicrosoftNETPlatformLibrary`게시 된 응용 프로그램에 ASP.NET Core 공유 프레임 워크 어셈블리가 포함되도록 매개 변수를 지정합니다 . 이미지에 공유 프레임 워크가 없기 때문에이 작업이 필요합니다.

이미지를 만들려면 `publish`폴더를 `s2i build`명령 의 소스 매개 변수로 전달합니다 . 이전과 같이 SDK 빌더 이미지를 사용할 수 있지만 애플리케이션이 이미 빌드되었으므로 대신 작은 런타임 이미지를 사용할 수 있습니다.

```bash
~$ s2i build bin/Release/netcoreapp2.1/publish mywebapp registry.redhat.io/dotnet/dotnet-21-runtime-rhel7:2.1 -p always
```



## 4. .NET Framework에서 .NET Core로 포팅하는 방법

### 4.1 종속성 분석

.NET Core에서 실행되는 경우 해당 응용 프로그램의 종속성과 응용 프로그램이 실행되지 않는 경우 필요한 종속성에 따라 응용 프로그램이 어떻게 달라지는 지에 대한 이해가 필요합니다.

[NuGet 패키지는](https://docs.microsoft.com/en-us/dotnet/core/porting/third-party-deps) 일반적으로 NuGet 웹 사이트에서 말하기 때문에 [쉽게 확인 할 수](https://docs.microsoft.com/en-us/dotnet/core/porting/third-party-deps) 있으며 패키지에는 각 플랫폼에 대한 폴더 세트가 있습니다. 패키지의 페이지에서 종속성을보고 다음 이름이있는 폴더 나 항목을 찾을 수도 있습니다.

```
netstandard1 . 0 
netstandard1 . 1 
netstandard1 . 2 
netstandard1 . 3 
netstandard1 . 4 
netstandard1 . 5 
netstandard1 . 6 
netcoreapp1 . 0 
potable - net45 - win8
potable - win8 - wpa8
potable - net451 - win81
potable - net45 - win8 - wpa8 - wpa81
```

종속성이 NuGet 패키지가 아닌 경우 [ApiPort 도구](https://github.com/Microsoft/dotnet-apiport/blob/master/docs/HowTo/) 는 종속성의 이식성을 검사 할 수 있습니다.

.NET Framework 2.0의 새로운 기능인 Compatibility Shim을 사용하면 .NET 표준을 사용하도록 전환되지 않은 .NET Framework 패키지를 참조 할 수 있습니다. 지원되지 않는 API를 사용하는 경우 여전히 문제가있을 수 있으므로 이러한 패키지를 철저히 테스트해야 합니다.



### 4.2 .NET 표준 라이브러리 대상 지정

.NET 표준 라이브러리는 모든 .NET 런타임에서 사용할 수 있도록 설계되었으므로 [.NET 표준 라이브러리를 대상으로하는](https://docs.microsoft.com/en-us/dotnet/core/porting/libraries#targeting-the-net-standard-library) 것이 크로스 플랫폼 클래스 라이브러리를 작성하는 가장 좋은 방법입니다.

8 개의 플랫폼에서 다양한 버전을 사용할 수 있습니다. 프로젝트가 하위 버전을 대상으로하는 경우 상위 버전을 대상으로하는 프로젝트를 참조 할 수 없습니다. 모든 프로젝트에서 사용할 수있는 가장 낮은 .NET Standard 버전을 선택하십시오. 다음은 실행되는 특정 영역을 보여주는 각 .NET Standard 버전의 차트입니다.

[MicroSoft .Net Standard Chart](https://docs.microsoft.com/en-us/dotnet/standard/net-standard)

| .NET Standard              | [1.0](https://github.com/dotnet/standard/blob/master/docs/versions/netstandard1.0.md) | [1.1](https://github.com/dotnet/standard/blob/master/docs/versions/netstandard1.1.md) | [1.2](https://github.com/dotnet/standard/blob/master/docs/versions/netstandard1.2.md) | [1.3](https://github.com/dotnet/standard/blob/master/docs/versions/netstandard1.3.md) | [1.4](https://github.com/dotnet/standard/blob/master/docs/versions/netstandard1.4.md) | [1.5](https://github.com/dotnet/standard/blob/master/docs/versions/netstandard1.5.md) | [1.6](https://github.com/dotnet/standard/blob/master/docs/versions/netstandard1.6.md) | [2.0](https://github.com/dotnet/standard/blob/master/docs/versions/netstandard2.0.md) |
| -------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| .NET Core                  | 1.0                                                          | 1.0                                                          | 1.0                                                          | 1.0                                                          | 1.0                                                          | 1.0                                                          | 1.0                                                          | 2.0                                                          |
| .NET Framework 1           | 4.5                                                          | 4.5                                                          | 4.5.1                                                        | 4.6                                                          | 4.6.1                                                        | 4.6.1                                                        | 4.6.1                                                        | 4.6.1                                                        |
| Mono                       | 4.6                                                          | 4.6                                                          | 4.6                                                          | 4.6                                                          | 4.6                                                          | 4.6                                                          | 4.6                                                          | 5.4                                                          |
| Xamarin.iOS                | 10.0                                                         | 10.0                                                         | 10.0                                                         | 10.0                                                         | 10.0                                                         | 10.0                                                         | 10.0                                                         | 10.14                                                        |
| Xamarin.Mac                | 3.0                                                          | 3.0                                                          | 3.0                                                          | 3.0                                                          | 3.0                                                          | 3.0                                                          | 3.0                                                          | 3.8                                                          |
| Xamarin.Android            | 7.0                                                          | 7.0                                                          | 7.0                                                          | 7.0                                                          | 7.0                                                          | 7.0                                                          | 7.0                                                          | 8.0                                                          |
| Universal Windows Platform | 10.0                                                         | 10.0                                                         | 10.0                                                         | 10.0                                                         | 10.0                                                         | 10.0.16299                                                   | 10.0.16299                                                   | 10.0.16299                                                   |
| Windows                    | 8.0                                                          | 8.0                                                          | 8.1                                                          |                                                              |                                                              |                                                              |                                                              |                                                              |
| Windows Phone              | 8.1                                                          | 8.1                                                          | 8.1                                                          |                                                              |                                                              |                                                              |                                                              |                                                              |
| Windows Phone Silverlight  | 8.0                                                          |                                                              |                                                              |                                                              |                                                              |                                                              |                                                              |                                                              |
| Unity                      | 2018.1                                                       | 2018.1                                                       | 2018.1                                                       | 2018.1                                                       | 2018.1                                                       | 2018.1                                                       | 2018.1                                                       | 2018.1                                                       |



### 4.3 프로젝트 재 타겟팅

포팅 된 모든 프로젝트는 [.NET Framework 4.6.2를 대상으로](https://docs.microsoft.com/en-us/dotnet/core/porting/libraries#retargeting-your-net-framework-code-to-net-framework-462) 해야합니다 . 이렇게하면 .NET Framework 특정 대상에 대한 API 대안이 지원되지 않는 API에 사용될 수 있습니다.

이 작업은 Visual Studio에서 "대상 프레임 워크"명령을 사용하여 쉽게 수행 할 수 있으며 프로젝트를 다시 컴파일 할 수 있습니다.



### 4.4 테스트 코드

.NET Core에 코드를 이식하는 것은 중요한 변화입니다. 테스트가 강력히 권장됩니다. 다음과 같은 [적합한 테스트 프레임 워크를 사용하십시오](https://docs.microsoft.com/en-us/dotnet/core/porting/libraries#porting-your-tests) .

- [xUnit](https://xunit.github.io/)
- [NUnit](http://nunit.org/)
- [MSTest](https://msdn.microsoft.com/library/ms243147.aspx)

xUnit과 같은 도구를 사용하면 템플릿을 사용하여 .NET Core 테스트를 작성할 수 있습니다. 다음 은 편집 된 .csproj 파일 의 [예](https://xunit.github.io/docs/getting-started-dotnet-core.html) 입니다.

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.NET.Test.Sdk" Version="15.9.0" />
    <PackageReference Include="xunit" Version="2.4.1" />
    <PackageReference Include="xunit.runner.visualstudio" Version="2.4.1" />
  </ItemGroup>

</Project>
```



### 4.5 포팅 실행

[코드를 이식하는 가장 좋은 방법](https://docs.microsoft.com/en-us/dotnet/core/porting/libraries#recommended-approach-to-porting) 은 프레임 워크의 구조에 따라 다릅니다. 그러나 코드 기반을 단계와 계층으로 나누면 잘 작동합니다.

1. 라이브러리의 "기본"을 식별하십시오. 이베이스는 다른 모든 것들이 사용해야하는 데이터 모델 또는 클래스 및 메소드 일 수 있습니다.
2. 새 .NET Core 프로젝트에베이스를 복사하십시오.
3. 코드를 컴파일하는 데 필요한 모든 변경을하십시오.
4. 다른 코드 레이어를 복사하고 반복하십시오.



## Appendix

### Xamarin

iOS, Android 또는 Windows Phone 장치에서 실행되는 앱을 개발하기위한 플랫폼입니다.

Xamarin은 C #으로 작성되었으며 모든 Visual Studio 에디션에 포함되어 있습니다.

Microsoft는 Xamarin이 사용자 인터페이스 (UI)를 만들고 여러 플랫폼의 응용 프로그램에서 성능을 최적화하는 가장 좋은 방법이라고 약속합니다. 앱이 적어도 iOS 및 Android 기기에서 실행되어야하는 시대에 중요합니다.

Xamarin [은 플랫폼간에 코드를 공유](https://www.altexsoft.com/blog/mobile/the-good-and-the-bad-of-xamarin-mobile-development/) 하고 단일 기술 스택을 사용하여 시장 출시 및 엔지니어링 비용을 줄입니다. 그러나 사용자 인터페이스가 많은 앱은 플랫폼 별 코딩이 더 필요합니다. 그런 다음 코드 공유 및 절감액이 감소합니다.



### 기타 .NET 표준 플랫폼

.NET Framework, .NET Core 및 Xamarin 외에도 .NET 표준은 다음 플랫폼을 지원합니다.

- [Mono](http://www.mono-project.com/) - Xamarin과 Microsoft가 공동 작업하기 전에 만든 오픈 소스 .NET입니다. 이는 C # 및 공용 언어 인프라에 대한 ECMA 표준을 기반으로합니다. C # 외에도 개발자는 VB 8, Java, Python, Ruby, Eiffel, F # 및 Oxygene을 사용할 수 있습니다.
- [Universal Windows Platform](https://docs.microsoft.com/en-us/windows/uwp/get-started/universal-application-platform-guide) - Windows 10, Windows 10 Mobile, Xbox One, IoT 및 Hololens 장치에서 실행되는 응용 프로그램 개발을 돕기 위해 Microsoft에서 만든 소프트웨어 플랫폼입니다. 개발자는 C ++, C #, VB.NET 및 XAML을 사용할 수 있습니다.
- Windows - 버전 8.0 및 8.1이 지원됩니다.
- [Windows Phone](https://msdn.microsoft.com/en-us/library/windows/apps/jj662921(v=vs.105).aspx) - Windows Phone은 주로 소비자 시장 용으로 개발되었으며 2015 년에는 Windows 10 Mobile로 대체되었습니다.
- [Windows Phone Silverlight](https://www.microsoft.com/silverlight/windows-phone/) - 더 이상 사용되지 않는 응용 프로그램 프레임 워크는 인터넷 응용 프로그램을 실행하고 Adobe Flash와 경쟁하도록 설계되었습니다.