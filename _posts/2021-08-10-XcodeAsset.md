---
layout: post
title: "[iOS] Xcode Asset 관련 개념"
date: 2021-08-10 19:34:10 +0700
excerpt: Asset Catalog/App Slicing/App Thining
categories: [iOS]
tags: [Xcode, Documentation]
---

> 참조
>
> - [https://www.boostcourse.org/mo326/lecture/16842/?isDesc=false](https://www.boostcourse.org/mo326/lecture/16842/?isDesc=false)
> - [애플 공식 문서 - Asset Catalogs](https://help.apple.com/xcode/mac/current/#/dev10510b1f7)
> - [애플 공식 문서 - Types Reference](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_ref-Asset_Catalog_Format/AssetTypes.html)

## 1️⃣ 에셋 카탈로그(Asset Catalog)

### 1️⃣ 에셋 카탈로그란?

Xcode에서 프로젝트를 생성하면 `Assets.xcassets` 폴더가 자동 생성되는데, 이 폴더에서 애플리케이션에 사용될 다양한 에셋을 관리하며, 이를 에셋 카탈로그라고 함.

에셋 카탈로그는 에셋과 다양한 디바이스의 속성에 대한 파일의 연결(mapping)을 통해서 애플리케이션 리소스에 쉽게 접근할 수 있도록 도와줌

​	(리소스: 실행 중일 때 사용되는 이미지, 음악 파일 등)

​	(속성: 디바이스의 특징, 사이즈 클래스, 주문형 리소스, 특정 타입의 정보를 포함함.)

### 2️⃣ 에셋 카탈로그 구성

에셋 카탈로그가 어떻게 구성되어 있으며, 각 요소는 어떤 역할을 하는지

<img src="https://cphinf.pstatic.net/mooc/20171230_74/1514625555079AlogH_PNG/figure1_1.png" width="50%"/>

**Groups**: 그룹은 한 개 이상의 또 다른 그룹이나 에셋을 가질 수 있음

**Assets**: 에셋은 한 가지 타입의 관련된 속성과 파일들의 집합을 나타냄.

**Asset name**: 에셋에 접근하기 위해 가발자가 정의한 문자열

**Asset files**: 선택한 에셋의 데이터 파일 또는 리소스

**Attributes**: 속성은 선택한 그룹, 에셋, 에셋파일의 속성을 나타냄.(인스펙터를 선택하면 볼 수 있음.)

<img src="https://cphinf.pstatic.net/mooc/20171230_2/1514625634102Cxdcv_PNG/asset_variation.png" width="50%"/>

**Asset variations**: 위 그림에서 선택된 하나의 조각(에셋 파일들의 집합)을 나타냄. 이 조각은 같은 속성 값(value)이 적용되는 단위

### 3️⃣ 에셋 카탈로그의 콘텐츠 타입

<img src ="https://cphinf.pstatic.net/mooc/20171230_268/151462568029977AD4_PNG/format_image.png" width="50%"/>

**Folders**: 에셋 카탈로그 폴더는 다른 그룹 폴더나 에셋 폴더를 포함할 수 있음. 파일 시스템의 폴더 이름은 대체적으로 확장자를 갖지 않지만, 에셋 카탈로그 폴더는 에셋 타입의 확장자가 자동으로 붙음.

**JSON files**: .json 확장자 파일, 속성에 대한 정보를 포함함.

**Contents files**: 콘텐츠 파일은 리소스 파일을 나타냄.



## 2️⃣ 앱 시닝(App Thining)과 앱 슬라이싱(App Slicing)

### 1️⃣ 앱 시닝이란?

애플리케이션이 디바이스에 설치될 때 앱스토어와 운영체제가 그 디바이스의 특성에 맞게 설치하도록 하는 설치 최적화 기술

- 이를 통해 애플리케이션의 설치용량⬇️, 다운로드 속도⬆️
- 주요 기술 구성요소: 슬라이싱(slicing), 비트코드(bitcode), 주문형 리소스(on-demand resource)

### 2️⃣ 슬라이싱(slicing)이란?

애플리케이션이 지원하는 다양한 디바이스에 대한 여러 조각의 애플리케이션 번들(app bundle)을 생성하고 디바이스에 알맞은 조각을 전달하는 기술

- 개발자가 애플리케이션의 전체 버전을 ***iTunes Connect**에 업로드 시, 앱 스토어에는 각 디바이스 특성에 다양한 버전의 조각들이 생성됨.

- 사용자가 애플리케이션ㅇ르 설치할 때, 전체 버전이 아닌 슬라이싱된 조각들 중 사용자의 디바이스의 가장 적합한 조각이 다운로드되어 설치됨.(에셋 카탈로그에서 관리하는 이미지들은 자동으로 적용됨.)

- 슬라이싱은 iOS 9.0 이상 버전만 지원

  (*iTunes Connect란 개발자가 앱 스토어에 판매할 애플리케이션을 제출하고 관리할 수 있도록 도와주는 웹 기반 도구)

<img src="https://cphinf.pstatic.net/mooc/20171230_100/1514625945492PozUu_PNG/slicing.png" width="50%">