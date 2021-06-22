---
layout: post
title: "[iOS] UIImagePickerController로 사진∙비디오 기록하기"
date: 2021-06-22 18:34:10 +0700
categories: [iOS]
---

## 0️⃣ TMI

기본 카메라 기능을 이용해서 비디오를 촬영하고 해당 비디오 컨텐츠에 접근하여 처리 프로세싱을 하고 싶었는데, CoreML Vision 모델에 적합한 카메라 프레임워크로 AVFoundation의 VideoCapture와 CoreVideo가 더 적합함. (그 이유는 다음 포스팅에 ㅎ) 

그래서 지금은 쓸모가 없어졌지만, 차후에 쓰일 날을 위해 UIImagePickerController 관련 포스팅 시작함니당



## 1️⃣ UIImagePickerController란?

사진 촬영, 동영상 녹화, 사용자의 미디어 라이브러리에서 항목 선택을위한 시스템 인터페이스를 관리하는 컨트롤러로, 우리가 흔히 아는 iOS 기본카메라 인터페이스를 실행시켜 사진 및 비디오 녹화 및 저장 앨범 접근을 가능하게 해준다.

``` swift
@MainActor class UIImagePickerController : UINavigationController
```



## 2️⃣ 코딩 전 해야 할 권한 요청

애플은 privacy에 민감하다. 그래서 카메라세션 관련 코드를 짜도 권한 요청을 설정해놓지 않으면 빌드에 실패한다.

`info.plist` 파일에서 아래 권한 Key 추가해주기

- Privacy - Camera Usage Description
- Privacy - Microphone Usage Description (동영상 녹화 시 필요)
- Privacy - Photo Library Additions Usage Description (앨범에 파일 저장 시 필요)

## 3️⃣ 코드

- 특정 버튼을 누르면 카메라가 켜질 수 있도록 액션함수 안에 코드를 작성하였다.

1. UIImagePickerController 객체 생성

2. 아이팟 등 `sourceType`을 지원하지 않을 수도 있어, `isSourceTypeAvailable`로 가능한지 확인 (2.1 가능하면 설정해주기) 

   - `sourceType` 종류

     ``` swift
     public enum UIImagePickerControllerSourceType : Int {
             case photoLibrary // 사진 앨범들
             case camera // 카메라를 띄우는 타입
             case savedPhotosAlbum // 특별한 순간
     }
     ```

3. UIImagePickerController에서 액세스 할 미디어유형을 결정하는 것으로, 사진∙비디오 둘 다 기록 가능
4. capture한 사진 또는 비디오에 편집할 수 있게 해줌.
5. present 함수로 기본 카메라 인터페이스 실행

``` swift
import UIKit
import MobileCoreServices

class ViewController: UIViewController {
    var controller = UIImagePickerController()		// 1
    
    // 시작하기 버튼 눌렀을 때 액션함수
    @IBAction func startButton(_ sender: Any) {
        if UIImagePickerController.isSourceTypeAvailable(.camera) {		// 2
            controller.sourceType = .camera		//2.1
            controller.mediaTypes = UIImagePickerController.availableMediaTypes(for: .camera) ?? []		// 3
            controller.allowsEditing = true    // 4
            controller.delegate = self
            
            present(controller, animated: true, completion: nil)		// 5
        } else {
            print("Camera is not available")
        }
    }

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
    }
}
```



## 4️⃣ 결과

1. UIImagePicker로 띄운 기본 카메라 인터페이스
   - 플래쉬 ON/OFF, 전면∙후면, 비디오∙사진 촬영 지원함.
2. 사진을 찍었을 때 크기 조절 가능한 뷰
3. 비디오를 찍었을 때 시작-끝 구간 조절 가능한 뷰

<img src="https://user-images.githubusercontent.com/47033052/122907939-95625b80-d38e-11eb-8406-49845b89fe26.PNG" width="30%"/> <img src="https://user-images.githubusercontent.com/47033052/122907902-8e3b4d80-d38e-11eb-92a1-7951af4f9616.PNG" width="30%"/> <img src="https://user-images.githubusercontent.com/47033052/122907931-94c9c500-d38e-11eb-82b1-e24207a4dfbc.PNG" width="30%"/>

- 라이브러리 접근하여 사진과 비디오 저장하는 법은 이번에 구현하지 않아서 다음에 기회가 되면 보충할 것이다..! 🌝

---

## 참조

- https://developer.apple.com/documentation/uikit/uiimagepickercontroller
- https://zeddios.tistory.com/949
- https://jinshine.github.io/2018/07/06/iOS/UIImagePickerController%20%EC%98%88%EC%A0%9C/

