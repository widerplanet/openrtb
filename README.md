Widerplanet RTB 연동 가이드
=======================

  * [1. Widerplanet 소개](#1-Widerplanet-소개)
    * [1.1 Widerplanet RTB](#11-Widerplanet-rtb)
    * [1.2 Widerplanet 연동 절차](#12-Widerplanet-연동-절차)
    * [1.3 Widerplanet 지원 배너 종류](#13-Widerplanet-지원-배너-종류)
  * [2. OpenRTB Basics](#2-openrtb-basics)
    * [2.1 입찰 요청](#21-입찰-요청)
    * [2.2 입찰 요청에 대한 응답](#22-입찰-요청에-대한-응답)
  * [3. 입찰 요청(Bid Request Specification)](#3-입찰-요청bid-request-specification)
    * [3.1 Object: BidRequest](#31-object-bidrequest)
    * [3.2 Object: Imp](#32-object-imp)
    * [3.3 Object: Banner](#33-object-banner)
    * [3.4 Object: Native](#34-object-native)
    * [3.5 Object: Site](#35-object-site)
    * [3.6 Object: App](#36-object-app)
    * [3.7 Object: Device](#37-object-device)
    * [3.8 Object: User](#38-object-user)
  * [4. 입찰 응답(Bid Response Specification)](#4-입찰-응답bid-response-specification)
    * [4.1 Object: BidResponse](#41-object-bidresponse)
    * [4.2 Object: SeatBid](#42-object-seatbid)
    * [4.3 Object: Bid](#43-object-bid)
    * [4.4 매크로 치환 (Substitution Macros)](#44-매크로-치환-Substitution-Macros)
  * [5. Native 규격](#5-native-규격)
    * [5.1 입찰 요청](#51-입찰-요청)
      * [5.1.1 Native Markup Request Object](#511-Native-Markup-Request-Object)
      * [5.1.2 Asset Request Object](#512-Asset-Request-Object)
        * [5.1.2.1 Title Request Object](#5121-Title-Request-Object)
        * [5.1.2.2 Image Request Object](#5122-Image-Request-Object)
        * [5.1.2.3 Data Request Object](#5123-Data-Request-Object)
    * [5.2 입찰 응답](#52-입찰-응답)
      * [5.2.1 Native Markup Response Object](#521-Native-Markup-Response-Object)
  * [6. 비딩 입찰 요청 응답 예제(Bid Request Response Samples)](#6-비딩-입찰-요청-응답-예제bid-request-response-samples)
    * [6.1 Bid Requests](#61-bid-requests)
      * [6.1.1 Example - 디스플레이 광고 요청](#611-example---디스플레이-광고-요청)
      * [6.1.2 Example - Native 광고 요청](#612-example---Native-광고-요청)
    * [6.2 Bid Responses](#62-bid-responses)
      * [6.2.1 Example - 디스플레이 광고 응답](#621-example---디스플레이-광고-응답)
      * [6.2.2 Example - Native 광고 응답](#622-example---Native-광고-응답)
  * [7. 쿠키교환 Cookie Matching - Cookie Sync](#7-쿠키교환-cookie-matching---cookie-sync)

<br/><br/>

# 1. Widerplanet 소개

## 1.1 Widerplanet RTB

* 본 문서에서는, Widerplanet과 매체간 OpenRTB 프로토콜을 통해 연동하는 방법을 안내합니다.  
* OpenRTB는 2010년 부터 IAB(Interactive Advertising Bureau)에서 제작되어 현재 까지 개발 진행 중이며, Widerplanet은 ***[OpenRTB API Specification Version 2.5](http://cdn-aitg.widerplanet.com/static/OpenRTB-API-Specification-Version-2-5-FINAL.pdf)*** , ***[OpenRTB Dynamic Native Ads API Specification Version 1.2](http://cdn-aitg.widerplanet.com/static/OpenRTB-Native-Ads-Specification-Final-1.2.pdf)*** 을 기반으로 합니다. 단, 모든 스펙이 구현되어 있지는 않으며, 연동시 추가로 필요한 스펙은 양사간 협의하에 추가 구현됩니다.


## 1.2 Widerplanet 연동 절차

 순서    | 내용                                                 | 담당
:-------|:----------------------------------------------------|:-------------------
 00     | 문의                                                 | 매체
 01     | Widerplanet 연동 가이드 및 매체 questionnaire 전달       | Widerplanet
 02     | Widerplanet 연동 가이드 검토 및 매체 questionnaire 작성   | 매체
 03     | 가능 여부 판단 후 디바이스 및 사이즈 협의                    | Widerplanet, 매체
 04     | 계약서 전달 및 날인                                     | Widerplanet, 매체
 05     | Web 매체 쿠키교환 Cookie Matching (사용자 ID 교환)        | Widerplanet, 매체
 06     | Test 캠페인 설정 및 EndPoint 및 응답 전문 전달            | Widerplanet, 매체
 07     | 응답 전문 검토 확인                                     | 매체
 08     | 테스트 연동 요청 시작                                    | Widerplanet, 매체
 09     | 모니터링                                              | Widerplanet, 매체
 10     | 테스트 종료 및 통계 정보 확인                             | Widerplanet, 매체
 11     | 상용 연동 시작                                         | Widerplanet, 매체



## 1.3 Widerplanet 지원 배너 종류
  * 디스플레이 배너
    * Widerplanet 에서 구현된 소재를 HTML (iframe or full static html) 형태로 전달
  * Native 배너
    * OpenRTB Native Spec 에 따라 랜더링에 필요한 정보를 전달 (매체측 랜더링 구현 필요)


<br/><br/>

# 2. OpenRTB Basics

아래 그림은 익스체인지와 비더간의 OpenRTB 상호작용을 나타내고 있습니다. 광고 요청은 매체 사이트에서 발생됩니다. 매 광고 요청마다 비딩 요청이 모든 비더들에게 전파되고 비더로 부터 온 응답들은 일반적인 경매 룰에 의해 평가되어 위너(Winner)는 경매 성공을 통보받고 광고 마크업(markup)이 익스체인지로 전달됩니다. 다른 상호작용(블럭리스트 동기, 통신 제어 등)은 다음 제안에 포함되거나 이미 OpenRTB에 정의 되어있습니다.

![OpenRTB-Basic](https://cdn-aitg.widerplanet.com/static/open_rtb_request_sequence.jpg)

## 2.1 입찰 요청
입찰 요청은 연동절차에 따라 부여받은 end-point 로 HTTP POST 프로토콜로 요청되어야 합니다.

```
// 요청 헤더 (필수)
Content-Type: application/json

// gzip 으로 요청문을 전달 할 경우 Content-Encoding 헤더 추가 필요
Content-Encoding: gzip
```

## 2.2 입찰 요청에 대한 응답

* No Bid(광고 없음)인 경우 HTTP 코드 204 리턴
* 입찰인 경우 HTTP 코드 200 과 함께 입찰을 위한 JSON repsponse body 리턴



<br/><br/>

# 3. 입찰 요청(Bid Request Specification)

RTB 시작은 입찰 요청을 보내면서 시작됩니다. BidRequest는 하나 이상의 Imp(impression) Object로 구성되며, Imp에 대한 추가 정보를 추가하여 연동합니다.

## 3.1 Object: BidRequest

입찰 최상위 오브젝트. 입찰 고유 값인 id, imp, app or site 의 필수 오브젝트가 포함되어야 합니다. 필요에 따라 device(기기정보) 오브젝트, 통화 종류를 나타내는 cur 값들, 또 테스트 입찰값인 test 등이 권장 사항으로 포함됩니다.

 Name   | Type         | 필수, 기본값  | Description                                                               
:-------|:-------------|:--------------|:--------------------------------------------------------------------------
 id     | string       | 필수           | 입찰에 대한 유니크 아이디                                                 
 imp    | object array | 필수           | imp 오브젝트 배열 (다중 요청인 경우 랜덤 하나에 대한 입찰응답)                                                        
 site   | object       | 필수(or app)   | app 개체와 site 오브젝트 둘 중 하나가 반드시 포함 되어야함.
 app    | object       | 필수(or site)  | app 개체와 site 오브젝트 둘 중 하나가 반드시 포함 되어야함.               
 device | object       |               | 광고가 전송될 디바이스 특성, 종류등을 설명합니다.                         
 user   | object       | 필수           | 사용자 정보 (웹 지면인 경우 Widerplanet 과 쿠키매칭 된 정보 필수)
 test   | integer      |               | 1인 경우 테스트 연동 - 매체비 과금하지 않음                    
 at     | integer      | 필수           | Auction Type 1: 1st-price auction, 2: 2nd-price auction
 cur    | string array | 필수           | ISO–4217 코드의 단위통화 리스트. - 계약시 USD, KRW 중 확정 필요
 bcat   | string array |               | 제외되어야 할 광고주 카테고리 리스트<br/>IAB OpenRTB Spec 2.5 > 표 5.1 조
 badv   | string array |               | 제외되어야 할 광고주의 최상위 도메인 리스트.                              

## 3.2 Object: Imp

입찰 대상이 되는 광고의 위치나 광고 종류 등을 나타냅니다.

 Name              | Type    | 필수, 기본값    | Description                                                  
:------------------|:--------|:----------------|:-------------------------------------------------------------
 id                | string  | 필수             | BidRequest 오브젝트 안에서 imp를 구분하기 위한 고유 식별자   
 banner            | object  | 필수             | banner, native 오브젝트중 하나 이상을 포함하고 있어야 합니다.
 native            | object  | 필수             | banner, native 오브젝트중 하나 이상을 포함하고 있어야 합니다.
 instl             | integer |                 | 전면광고 여부. 1일 경우 전면                                 
 tagid             | string  | 필수             | 노출 인벤토리(해당 지면, 유닛)의 고유한 식별자               
 bidfloor          | integer |                 | Impression의 입찰 최저가                                     
 bidfloorcur       | string  | 필수             | 계약시 USD, KRW 중 확정 필요         


## 3.3 Object: Banner

디스플레이 광고. native 가 아닌 일반 광고일 경우 반드시 포함되어야 합니다.

 Name     | Type          | 필수, 기본값 | Description                                                                          
:---------|:--------------|:-------------|:-------------------------------------------------------------------------------------
 w        | integer       | 필수          | 광고의 넓이(pixel)                                                                   
 h        | integer       | 필수          | 광고의 높이(pixel)                                                                   
 btype    | integer array |              | 제외 되어야 할 광고소재 종류<br>IAB OpenRTB Spec 2.5 > 표 5.2 참조                     
 battr    | integer array |              | 제외 되어야 할 광고소재 속성<br>IAB OpenRTB Spec 2.5 > 표 5.3 참조                     
         

## 3.4 Object: Native

Native 형식의 Impression을 나타냅니다. Open RTB Native Spec에 의해 요청된 소재정보를 입찰시 응답 합니다.

 Name    | Type          | 필수, 기본값 | Description                                                                                        
:--------|:--------------|:-------------|:---------------------------------------------------------------------------------------------------
 request | string        | 필수          | Native Ad Spec 을 준수하는 요청 페이로드<br>Open RTB Native Spec 에 의거한 Json stirng
 ver     | string        |              | Native Ad Spec 버전


## 3.5 Object: Site

광고가 전송될 지면이 웹사이트일 경우 반드시 포함되어야 합니다. site오브젝트와 app오브젝트와 동시에 포함 할 수 없습니다.

 Name       | Type         | 필수, 기본값 | Description                                                    
:-----------|:-------------|:-------------|:---------------------------------------------------------------
 id         | string       | 필수          | 사이트 ID                                    
 name       | string       | 필수          | 사이트 이름                                                    
 domain     | string       | 필수          | 사이트 도메인. 광고주에서 블럭 처리하는데 사용 할 수 있습니다.
 cat        | string array |              | 전체 IAB 카테고리 리스트                                       
 sectioncat | string array |              | 현재 섹션의 IAB 카테고리 리스트                                
 mobile     | integer      |              | 모바일 최적화 여부 0 = no, 1 = yes                             
 publisher  | object       |              | publisher 상세 정보        
 content    | object       |              | content 상세 정보                                        

## 3.6 Object: App

광고가 전송될 지면이 어플리케이션일 경우 반드시 포함되어야 합니다. site오브젝트와 app오브젝트와 동시에 포함 할 수 없습니다.

 Name       | Type         | 필수, 기본값 | Description                                               
:-----------|:-------------|:-------------|:----------------------------------------------------------
 id         | string       | 필수          | 앱 ID                                    
 name       | string       | 필수          | 앱 이름                                                   
 bundle     | string       | 필수          | Android 패키지명, IOS에서는 패키지명 혹은 어플리케이션 ID
 domain     | string       |              | 어플리케이션 도메인.                                      
 storeurl   | string       | 권장          | 앱스토어 URL                                              
 cat        | string array |              | 전체 IAB 카테고리 리스트                                  
 sectioncat | string array |              | 현재 섹션의 IAB 카테고리 리스트                           
 ver        | integer      |              | 어플리케이션 버전                                         
 paid       | integer      |              | 0 = 무료, 1 = 유료                                        
 publisher  | object       |              | publisher 상세 정보 
 content    | object       |              | content 상세 정보                                   


## 3.7 Object: Device

하드웨어, 플랫폼, 위치, 통신사등 해당 기기와 관련되 정보를 제공합니다.

 Name           | Type    | 필수, 기본값 | Description                                                     
:---------------|:--------|:-------------|:----------------------------------------------------------------
 ua             | string  | 필수          | User Agent - 지면이 웹인 경우 필수                                                     
 geo            | object  |              | 위치 정보                                                       
 ip             | string  | 필수          | IPv4 주소                                                       
 devicetype     | integer |              | 디바이스 종류. IAB OpenRTB Spec 2.5 > 표 5.17 참조              
 make           | string  |              | 디바이스 제조사                                                 
 model          | string  |              | 디바이스 모델                                                   
 os             | string  | 필수          | 디바이스 운영체제 (android, ios)                                
 osv            | string  |              | 디바이스 운영체제 버전                                          
 h              | integer |              | 디바이스 넓이(pixel)                                            
 w              | integer |              | 디바이스 높이(pixel)                                            
 language       | string  |              | 브라우저 언어. ISO-639-1-alpha-2                                
 carrier        | string  |              | 통신사 또는 IP 어드레스로 부터 유도된 ISP(인터넷 서비스 제공자)
 connectiontype | string  |              | 네트워크 연결 종류 IAB OpenRTB Spec 2.5 > 표 5.18 참조          
 ifa            | string  | 필수          | 광고 트래킹 아이디(ex) android = gaid, ios = idfa) - 지면이 앱인 경우 필수                  


## 3.8 Object: User

디바이스 사용자의 정보를 나타냅니다.

 Name     | Type    | 필수, 기본값 | Description                                                                        
:---------|:--------|:-------------|:-----------------------------------------------------------------------------------
 id       | string  | 필수          | Widerplanet 사용자 ID - 지면이 웹인 경우 id 또는 buyerid 값은 필수
 buyeruid | string  | 필수          | Widerplanet 사용자 ID - 지면이 웹인 경우 id 또는 buyerid 값은 필수


<br/><br/>

# 4. 입찰 응답(Bid Response Specification)

## 4.1 Object: BidResponse

 Name    | Type         | 필수, 기본값 | Description                                                                              
:--------|:-------------|:-------------|:-----------------------------------------------------------------------------------------
 id      | string       | 필수         | Bid Request의 ID                                                                         
 seatbid | object array | 필수         |                                                                                          
 bidid   | string       |              | 응답의 ID로 입찰자가 응답을 추적하기 위해 사용함. 입찰자에 의해 선택됩니다.              
 cur     | string       | 필수         | ISO–4217 코드의 단위통화.                                                                

최상위 오브젝트로 id는 BidRequest의 ID를 그대로 사용합니다. 최소 하나의 seatbid 오브젝트가 필수이며, 하나의 imp에 대한 입찰을 포함합니다.

## 4.2 Object: SeatBid

 Name | Type         | 필수, 기본값 | Description                                                                                                                                   
:-----|:-------------|:-------------|:--------------------------------------------------
 bid  | object array | 필수         | imp 대한 응답 오브젝트                                                                                                                        
 seat | string       |              | 입찰하는 입찰 자격 코드                                                                                                                       

## 4.3 Object: Bid

 Name    | Type         | 필수, 기본값 | Description                                             
:--------|:-------------|:-------------|:-----------------------------------------------------------------------------------------
 id      | string       | 필수         | 입찰자가 트래킹에 사용할 유니크한 아이디  
 impid   | string       | 필수         | 응답에 대한 요청 Imp 오브젝트의 아이디
 price   | float        | 필수         | CPM 단위의 입찰 
 adid    | string       |              | 낙찰시 전송될 광고 ID 
 nurl    | string       |              | 낙찰시 통보 URL
 lurl    | string       |              | 유찰시 통보 URL
 adm     | string       |              | 광고 마크업 
 adomain | string array | 필수         | 광고주 최상위 도메인(광고주 필터링에 사용) 
 bundle  | string       |              | 어플리케이션일 경우 패키지 명(앱광고등)
 iurl    | string       | 필수         | 광고 컨텐츠 확익을 위한 샘플 이미지 URL
 cid     | string       | 필수         | 광고 캠페인 ID
 crid    | string       | 필수         | 광고소재 ID
 cat     | string array |              | 광고소재의 컨텐츠 카테고리 목록. IAB OpenRTB Spec 2.5 > 표 5.1 참조 
 attr    | string array |              | 광고소재 속성. OpenRTB Spec 2.5 > 표 5.3 참조  
 w       | integer      |              | 광고소재 넓이(pixel)
 h       | integer      |              | 광고소재 높이(pixel)

## 4.4 매크로 치환 (Substitution Macros)

입찰 시 Widerplanet 은 낙찰통보(Win Notice) URL 을 ${AUCTION_PRICE} 매크로를 포함하여 응답합니다. 
낙찰통보 를 받을 Object 는 계약시 burl, nurl 등으로 사전에 지정되어야 하며, 낙찰시 매체에서는 매크로 값을 치환하여 호출해 주어야 합니다.

 Macro                   | Description                                               
:------------------------|:----------------------------------------------------------
 ${AUCTION_PRICE}        | 낙찰 가격 (매체에서는 낙찰시 매크로 치환 후 호출 필요)


<br/><br/>

# 5. Native 규격

Widerplanet Native는 OpenRTB-Native-Ads-Specification-Final-1.2 를 기본으로 구성되었습니다.

## 5.1 입찰 요청

입찰 요약 규격(상세 정보는 OpenRTB-Native-Ads-Specification 1.2 참조)

## 5.1.1 Native Markup Request Object
 Name           | Type         | 필수, 기본값 | Description                                                                              
:---------------|:-------------|:-------------|:-----------------------------------------------------------------------------------------
 ver            | string       |              | 네이티브 요청 마크업 버전
 assets         | object array | 필수          | 입찰에 필요한 네이티브 구성요소를 정의한 assets 객체                    
 eventtrackers  | object array |              | 네이티브 이벤트 트래커 객체 - 계약시 사용할 이벤트 확정 필요
                                         

## 5.1.2 Asset Request Object
 Name           | Type         | 필수, 기본값 | Description                                                                              
:---------------|:-------------|:-------------|:-----------------------------------------------------------------------------------------
 id             | integer      | 필수          | 고유한 asset id
 required       | integer      |              | 1인경우 필수값
 title          | object       | 권장          | 타이틀 요청 asset 객체
 image          | object       | 권장          | 이미지 요청 asset 객체
 data           | object       |              | 데이타 요청 asset 객체

## 5.1.2.1 Title Request Object
 Name           | Type         | 필수, 기본값 | Description                                                                              
:---------------|:-------------|:-------------|:-----------------------------------------------------------------------------------------
 len            | integer      | 필수          | 최대 타이틀 길이 - 광고 타이틀이 len 보다 긴 경우 "..." 처리

## 5.1.2.2 Image Request Object
 Name           | Type         | 필수, 기본값 | Description                                                                              
:---------------|:-------------|:-------------|:-----------------------------------------------------------------------------------------
 type           | integer      | 필수          | 이미지 타입<br/>1: 로고 또는 앱 아이콘<br/>3: 메인 이미지
 w              | integer      | 권장          | 이미지 width
 h              | integer      | 권장          | 이미지 height
 wmin           | integer      |              | 이미지 최소 width
 hmin           | integer      |              | 이미지 최소 height

## 5.1.2.3 Data Request Object
 Name           | Type         | 필수, 기본값 | Description                                                                              
:---------------|:-------------|:-------------|:-----------------------------------------------------------------------------------------
 type           | integer      | 필수          | 데이터 타입<br/>1: ADVERTISER<br/>2: DESCRIPTION<br/>12: CTA_DESCRPTION (버튼문구)<br/>500: OptOut 링크<br/>501: OptOut 아이콘
 len            | integer      |              | 데이터 최대 길이 - 데이터 문자열이 len 보다 긴 경우 "..." 처리



## 5.2 입찰 응답

입찰 응답 규격(상세 정보는 OpenRTB-Native-Ads-Specification 1.2 참조)

## 5.2.1 Native Markup Response Object
 Name           | Type         | 필수, 기본값 | Description                                                                              
:---------------|:-------------|:-------------|:-----------------------------------------------------------------------------------------
 ver            | string       |              | 네이티브 응답 마크업 버전
 assets         | object array | 필수          | 입찰에 사용될 네이티브 구성요소를 정의한 assets 객체                    
 link           | object array | 필수          | 광고 클릭정보를 담고있는 link 객체 (클릭 시 link>url 주소를 호출해야함)
 eventtrackers  | object array |              | 네이티브 이벤트 트래커 객체 - 계약시 사용할 이벤트 확정 필요
 imptrackers    | string array |              | 네이티브 노출 트래커 리스트 - 계약시 사용 여부 확정 필요
    

<br/><br/>


# 6. 비딩 입찰 요청 응답 예제(Bid Request Response Samples)

## 6.1 Bid Requests

### 6.1.1 Example - 디스플레이 광고 요청

```json
{
    "id": "b680210a-17b4-4650-8bd9-f99c89c74e3a",
    "imp": [
        {
            "id": "1eb3fd3b-b52e-4f33-8474-3762efdcb64d",
            "instl": 0,
            "tagid": "f9278757",
            "secure": 1,
            "exp": 1800,
            "banner": {
                "w": 320,
                "h": 50,
                "btype": [
                ],
                "format": [
                    {
                        "w": 320,
                        "h": 50
                    }
                ],
                "pos": 0
            },
            "bidfloor": 0.0809688576,
            "bidfloorcur": "USD"
        }
    ],
    "device": {
        "carrier": "SK Broadband",
        "ua": "Dalvik/2.1.0 (Linux; U; Android 7.0; LGM-G600S Build/NRD90U)",
        "ip": "218.39.94.47",
        "language": "ko",
        "make": "LG",
        "model": "LGM-G600S",
        "w": 320,
        "h": 50,
        "os": "Android",
        "osv": "7.0",
        "devicetype": 4,
        "ifa": "894dfcb3-8ac4-43c0-a69c-012345678901",
        "js": 1,
        "connectiontype": 2,
        "dpidsha1": "700000000230297ba2108232e84c23d952ad5eed",
        "dpidmd5": "4bcca03eee7b0e00440asd2342342342",
        "geo": {
            "lat": 37.5985,
            "lon": 126.9783,
            "type": 2,
            "city": "Seoul",
            "zip": "02878",
            "country": "KOR",
            "region": "11"
        },
        "dnt": 0,
        "lmt": 0
    },
    "app": {
        "id": "1233434",
        "name": "앱 이름",
        "bundle": "com.testapp",
        "storeurl": "https://play.google.com/store/apps/details?id=com.testapp",
        "keywords": "Massenger&Community",
        "cat": [
            "IAB3"
        ],
        "publisher": {
            "name": "(주)매체",
            "id": "a345434e-31ba-488c-939c-290c48d577e4"
        }
    },
    "allimps": 0,
    "cur": [
        "USD"
    ],
    "tmax": 234,
    "badv": [
        "block.com",
        "adv_test.com"
    ],
    "at": 2
}
```

### 6.1.2 Example - Native 광고 요청

```json
{
    "id": "a94aa51e-0940-4ab6-9f10-bd5ce9decbab",
    "imp": [
        {
            "id": "23423432-588c-445d-b390-3b81a1789c4d",
            "instl": 0,
            "tagid": "9999343-2-526496",
            "secure": 1,
            "exp": 1800,
            "native": {
                "ver": "1.1",
                "request": "{\"native\":{\"ver\":\"1.1\",\"plcmttype\":1,\"plcmtcnt\":1,\"seq\":0,\"assets\":[{\"id\":1,\"required\":0,\"data\":{\"type\":12,\"len\":1000}},{\"id\":2,\"required\":1,\"title\":{\"len\":100}},{\"id\":3,\"required\":1,\"img\":{\"type\":1,\"w\":80,\"wmin\":0,\"h\":80,\"hmin\":0}},{\"id\":4,\"required\":1,\"img\":{\"type\":3,\"w\":1200,\"wmin\":0,\"h\":627,\"hmin\":0}},{\"id\":5,\"required\":0,\"data\":{\"type\":3,\"len\":1000}},{\"id\":6,\"required\":1,\"data\":{\"type\":2,\"len\":150}}]}}"
            },
            "bidfloor": 0.0771241824,
            "bidfloorcur": "USD"
        }
    ],
    "device": {
        "carrier": "SK Telecom",
        "ua": "Mozilla/5.0 (Linux; U; Android 10; ko_KR; SM-N971N Build/unknown) AppleWebKit/535.30 (KHTML, like Gecko) Chrome/18.0.1025.133 Mobile Safari/535.19",
        "ip": "223.39.202.129",
        "language": "ko",
        "make": "Samsung",
        "model": "SM-N971N",
        "os": "Android",
        "osv": "10",
        "devicetype": 4,
        "ifa": "494cf0e4-99fc-4800-bc34-012345678901",
        "js": 1,
        "connectiontype": 3,
        "dpidsha1": "700000000230297ba2108232e84c23d952ad5eed",
        "dpidmd5": "4bcca03eee7b0e00440asd2342342342",
        "geo": {
            "lat": 37.5985,
            "lon": 126.9783,
            "type": 2,
            "city": "Seoul",
            "zip": "02878",
            "country": "KOR",
            "region": "11"
        },
        "dnt": 0,
        "lmt": 0
    },
    "app": {
        "id": "1233434",
        "name": "앱 이름",
        "bundle": "com.testapp",
        "storeurl": "https://play.google.com/store/apps/details?id=com.testapp",
        "keywords": "Massenger&Community",
        "cat": [
            "IAB3"
        ],
        "publisher": {
            "name": "(주)매체",
            "id": "a345434e-31ba-488c-939c-290c48d577e4"
        }
    },
    "allimps": 0,
    "cur": [
        "USD"
    ],
    "tmax": 1984,
    "bcat": [
        "IAB18-3",
        "IAB22-1",
        "IAB22-2",
        "IAB22-4",
        "IAB18",
        "IAB24",
        "IAB22-3",
        "IAB22",
        "IAB13",
        "IAB26-1",
        "IAB26",
        "IAB1"
    ],
    "badv": [
        "block.com",
        "adv_test.com"
    ],
    "at": 1
}
```


## 6.2 Bid Responses

### 6.2.1 Example - 디스플레이 광고 응답

```json
{
  "id": "a94aa51e-0940-4ab6-9f10-bd5ce9decbab",
  "seatbid": [
    {
      "bid": [
        {
          "id": "4249acf3de5c84dc10542e2dfb34202216057703880280002874",
          "impid": "1eb3fd3b-b52e-4f33-8474-3762efdcb64d",
          "price": 0.09183216492909131,
          "nurl": "https://algd.widerplanet.com/delivery/win.php?currid=h&shd_id=2&engine=3.0&v=1&zoneid=00000&lid=47350&dlid=4249acf3de5c84dc10542e2dfb34202216057703880280002874&appid=com.kscc.scxb.mbl&rvt=2&gpr=2s&v_resp=2.1&dmpc=1&dmpsc=49864&dmpsp=0&geotg=KR0110000&os=android&appid=com.kscc.scxb.mbl&zct=1&cb=f169367692&dtype=display&ctype=200&bannerid=4137482&campaignid=331520&rv=6w2m22o&orv=6w2m22o&cid=4949184&qsc=s1abd6&wp=${AUCTION_PRICE}&category=f9278757&render_type=display&ci_c=10&bst=1&bsui=-hosoLp4Y_CCVBWL-xEPOzk9ooeL5lioRH_ZywI-gS-dWyl1XH0VDYT99GAE6qrNnbglYsrpPgTxNqfmc3ZUoXcVZQEu1-rrFtJXBnWXzbyGSx_FdE10wGWXyho5-1KB9Ly08uqe8HCK-raY_58e3gtdCHNpHLeot_EXypUkjl4.&sl=foobar&eb=KR&er2=MC4wMDA5NzcwNTM0Mg==&ebt=6",
          "adm": "<iframe src=\"https://algd.widerplanet.com/delivery/rad.php?currid=h&shd_id=2&engine=3.0&v=1&zoneid=00000&lid=47350&dlid=4249acf3de5c84dc10542e2dfb34202216057703880280002874&appid=com.kscc.scxb.mbl&rvt=2&gpr=2s&v_resp=2.1&dmpc=1&dmpsc=49864&dmpsp=0&geotg=KR0110000&os=android&appid=com.kscc.scxb.mbl&zct=1&cb=f169367692&cid=331520&crid=4949184&c_type=200&d_type=display&ad_id=4137482&rv=6w2m22o&orv=6w2m22o&qsc=tti8pa&wp=${AUCTION_PRICE}&category=f9278757&render_type=display&ci_c=10&bst=1&bsui=-hosoLp4Y_CCVBWL-xEPOzk9ooeL5lioRH_ZywI-gS-dWyl1XH0VDYT99GAE6qrNnbglYsrpPgTxNqfmc3ZUoXcVZQEu1-rrFtJXBnWXzbyGSx_FdE10wGWXyho5-1KB9Ly08uqe8HCK-raY_58e3gtdCHNpHLeot_EXypUkjl4.&sl=foobar&eb=KR&er2=MC4wMDA5NzcwNTM0Mg==&ebt=6&eb=KR\" width=\"320\" height=\"50\" scrolling=\"no\" border=\"0\" frameborder=\"0\"></iframe>",
          "adomain": [
            "ohou.se"
          ],
          "bundle": "net.testapp",
          "iurl": "https://cdn-aitg.widerplanet.com/images/wp/thumb_h/c0/7e/47350_400000.jpg",
          "cid": "331520",
          "crid": "47350_4949184_320x50__KOR",
          "cat": [
            "IAB10-7",
            "IAB10-8",
            "IAB10-9"
          ],
          "w": 320,
          "h": 50
        }
      ]
    }
  ],
  "bidid": "4249acf3de5c84dc10542e2dfb34202216057703880280002874",
  "cur": "USD"
}
```

### 6.2.2 Example - Native 광고 응답

```json
{
  "id": "a94aa51e-0940-4ab6-9f10-bd5ce9decbab",
  "seatbid": [
    {
      "bid": [
        {
          "id": "4249acf3de5c84dc10542e2dfb34202216057706612450000542",
          "impid": "e8a6f17a-588c-445d-b390-3b81a1789c4d",
          "price": 0.09335114658043706,
          "nurl": "https://algd.widerplanet.com/delivery/win.php?currid=h&shd_id=4&engine=3.0&v=1&zoneid=00000&lid=43416&dlid=4249acf3de5c84dc10542e2dfb34202216057706612450000542&appid=com.wafour.wapicjapanese&rvt=2&gpr=2s&v_resp=2.1&dmpc=1&dmpsc=49862&dmpsp=0&geotg=KR0370000&os=android&appid=com.wafour.wapicjapanese&zct=1&cb=29b232e319&dtype=display&ctype=201&bannerid=4218946&campaignid=337216&rv=2aovcow&cid=5051008&qsc=xswzhc&wp=${AUCTION_PRICE}&category=1530506-2-526496&render_type=native&ci_c=10&bst=1&bsui=mfnoI7eJ6ghGHT-oF8ycGa2yyChQOwOTj0Uc9HZuVa6E4B8iyIfMdrgROKy4RH-ONeOWXS8TY3DSXUaWB7GHciGAjWOO91lWn2K_cwR6vwD7BoM2V-4eH2xXTIzFme5CnVhzpezd5IqHLH5bstNJ6bl4nfC6f5p4PZ-Nw0G96L4.&sl=foobar&eb=KR&er2=MC4wMDI1OTA5MzI2NQ==&ebt=0",
          "adm": "{\"native\":{\"ver\":\"1.2\",\"assets\":[{\"id\":1,\"required\":0,\"data\":{\"value\":\"더 알아보기\",\"len\":6,\"type\":12}},{\"id\":2,\"title\":{\"text\":\"겨울방학  최상위권 결정전\"}},{\"id\":3,\"img\":{\"url\":\"https://cdn-aitg.widerplanet.com/images/ci/default_ci_300x300.png\",\"type\":1}},{\"id\":4,\"img\":{\"url\":\"https://cdn-aitg.widerplanet.com/images/wp/05/17/05d8013c9a129785373d319ef7f01117.jpg\",\"w\":1200,\"h\":627,\"type\":3}},{\"id\":6,\"required\":0,\"data\":{\"value\":\"테스트 광고 2021 고고고고\",\"len\":19,\"type\":2}}],\"link\":{\"url\":\"https://algd.widerplanet.com/delivery/ck.php?oaparams=2__currid=h__shd_id=4__engine=3.0__v=1__zoneid=00000__lid=43416__dlid=4249acf3de5c84dc10542e2dfb34202216057706612450000542__appid=com.wafour.wapicjapanese__rvt=2__gpr=2s__v_resp=2.1__dmpc=1__dmpsc=49862__dmpsp=0__geotg=KR0370000__os=android__appid=com.wafour.wapicjapanese__zct=1__cb=29b232e319__dtype=display__ctype=201__bannerid=4218946__campaignid=337216__rv=2aovcow__cid=5051008__oadest=http%3A%2F%2Fm.h4.hyperacademy.co.kr%2Fbranch%2Frecruit%2FdetailMain.do%3Frecruit_seq%3D375%26utm_source%3DMO_TG%26utm_medium%3Ddisplay%26utm_campaign%3DKEYWORD_A&qsc=40o1yk&wp=${AUCTION_PRICE}&category=1530506-2-526496&render_type=native&ci_c=10&bst=1&bsui=mfnoI7eJ6ghGHT-oF8ycGa2yyChQOwOTj0Uc9HZuVa6E4B8iyIfMdrgROKy4RH-ONeOWXS8TY3DSXUaWB7GHciGAjWOO91lWn2K_cwR6vwD7BoM2V-4eH2xXTIzFme5CnVhzpezd5IqHLH5bstNJ6bl4nfC6f5p4PZ-Nw0G96L4.&sl=foobar&eb=KR&er2=MC4wMDI1OTA5MzI2NQ==&ebt=0\"},\"imptrackers\":[\"https://algd.widerplanet.com/delivery/lg.php?currid=h&shd_id=4&engine=3.0&v=1&zoneid=00000&lid=43416&dlid=4249acf3de5c84dc10542e2dfb34202216057706612450000542&appid=com.wafour.wapicjapanese&rvt=2&gpr=2s&v_resp=2.1&dmpc=1&dmpsc=49862&dmpsp=0&geotg=KR0370000&os=android&appid=com.wafour.wapicjapanese&zct=1&cb=86cc974da1&dtype=display&ctype=201&bannerid=4218946&campaignid=337216&rv=2aovcow&cid=5051008&qsc=1lxtzmi&wp=${AUCTION_PRICE}&category=1530506-2-526496&render_type=native&ci_c=10&bst=1&bsui=mfnoI7eJ6ghGHT-oF8ycGa2yyChQOwOTj0Uc9HZuVa6E4B8iyIfMdrgROKy4RH-ONeOWXS8TY3DSXUaWB7GHciGAjWOO91lWn2K_cwR6vwD7BoM2V-4eH2xXTIzFme5CnVhzpezd5IqHLH5bstNJ6bl4nfC6f5p4PZ-Nw0G96L4.&OXLIA=1&sl=admixereu&eb=KR&er2=MC4wMDI1OTA5MzI2NQ==&ebt=0\"]}}",
          "adomain": [
            "www.advertiser.com"
          ],
          "iurl": "https://cdn-aitg.widerplanet.com/images/wp/05/17/05d8013c0000129785373d319ef7f01117.jpg",
          "cid": "337216",
          "crid": "43416_5051008_0x0__KOR",
          "cat": [
            "IAB5-6",
            "IAB6-6",
            "IAB6-8",
            "IAB5-13",
            "IAB5-14",
            "IAB5-1",
            "IAB14-6",
            "IAB5-10",
            "IAB5-11",
            "IAB4-11"
          ],
          "w": 0,
          "h": 0
        }
      ]
    }
  ],
  "bidid": "4249acf3de5c84dc10542e2dfb34202216057706612450000542",
  "cur": "USD"
}
```

<br/><br/>

# 7. 쿠키교환 Cookie Matching - Cookie Sync

매체 지면이 웹 사이트인 경우 매체는 Widerplanet 사용자 ID로 입찰요청을 해야합니다.

단, adverting id (Google ADID, Applie IDFA) 를 필수로 가지고 있는 앱 메체인 경우 쿠키교환은 불필요합니다.

쿠키교환과 입찰요청은 아래와 같은 flow 로 진행 됩니다.


순서    | 내용                                                                                                          | 담당
:-------|:-------------------------------------------------------------------------------------------------------------|:-------------------
 1     | 매체는 쿠키교환을 위한 end-point 를 Widerplanet 으로 제공합니다 (https 필수)                                        | 매체
 2     | Widerplanet 사용자가 광고주 사이트에 나타난 경우, 제공받은 end-point 로 "Widerplanet 사용자 ID" 추가하여 GET 방식으로 요청합니다.<br/>(hidden image 테그 호출방식) | Widerplanet
 3     | 매체는 전달받은 "Widerplanet 사용자 ID" 와 "매체 사용자 ID" 를 매체측 Database 에 저장합니다.                                  | 매체
 4     | 매체측 사용자가 네트워크에 나타난 경우, 매체측 Database 에 저장된 "Widerplanet 사용자 ID" 를 포함하여 Widerplanet 으로 입찰 요청합니다.    | 매체



 