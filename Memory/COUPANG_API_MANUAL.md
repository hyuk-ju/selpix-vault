---
type: research
note_status: fleeting
confidence_level: low
source_agent: unknown
created: 2026-02-19
---
# Coupang Wing API Documentation (Manual Ingestion)

Source: User Provided (from https://developers.coupangcorp.com/hc/ko/sections/360005046534)
Date: 2026-02-06

## 1. Product Creation (상품 생성)
**Endpoint**: `POST /v2/providers/seller_api/apis/api/v1/marketplace/seller-products`

### Request Body Example
```json
{
  "displayCategoryCode": 56137,
  "sellerProductName": "test_클렌징오일_관리용_상품명",
  "vendorId": "A00012345",
  "saleStartedAt": "2017-11-30T00:00:00",
  "saleEndedAt": "2099-01-01T23:59:59",
  "displayProductName": "해피바스 솝베리 클렌징 오일",
  "brand": "해피바스",
  "generalProductName": "솝베리 클렌징 오일",
  "productGroup": "클렌징 오일",
  "deliveryMethod": "SEQUENCIAL",
  "deliveryCompanyCode": "KDEXP",
  "deliveryChargeType": "FREE",
  "deliveryCharge": 0,
  "freeShipOverAmount": 0,
  "deliveryChargeOnReturn": 2500,
  "remoteAreaDeliverable": "N",
  "unionDeliveryType": "UNION_DELIVERY",
  "returnCenterCode": "1000274592",
  "returnChargeName": "반품지_1",
  "companyContactNumber": "02-1234-678",
  "returnZipCode": "135-090",
  "returnAddress": "서울특별시 강남구 삼성동",
  "returnAddressDetail": "333",
  "returnCharge": 2500,
  "outboundShippingPlaceCode": "74010",
  "vendorUserId": "wing_loginId_123",
  "requested": true,
  "items": [
    {
      "itemName": "200ml_1개",
      "originalPrice": 13000,
      "salePrice": 10000,
      "maximumBuyCount": "100",
      "maximumBuyForPerson": "0",
      "outboundShippingTimeDay": "1",
      "maximumBuyForPersonPeriod": "1",
      "unitCount": 1,
      "adultOnly": "EVERYONE",
      "taxType": "TAX",
      "parallelImported": "NOT_PARALLEL_IMPORTED",
      "overseasPurchased": "NOT_OVERSEAS_PURCHASED",
      "pccNeeded": "false",
      "externalVendorSku": "0001",
      "barcode": "",
      "emptyBarcode": true,
      "emptyBarcodeReason": "상품확인불가_바코드없음사유",
      "modelNo": "171717",
      "extraProperties": {
        "coupangSalePrice": 5000,
        "onlineSalePriceForBooks": 10000,
        "transactionType": "manufacturer",
        "businessType": "Beauty"
      },
      "certifications": [
        {
          "certificationType": "NOT_REQUIRED",
          "certificationCode": ""
        }
      ],
      "searchTags": [
        "검색어1",
        "검색어2"
      ],
      "images": [
        {
          "imageOrder": 0,
          "imageType": "REPRESENTATION",
          "vendorPath": "http://image11.coupangcdn.com/image/product/image/vendoritem/2018/06/25/3719529368/27a6b898-ff3b-4a27-b1e4-330a90c25e9c.jpg"
        },
        {
          "imageOrder": 1,
          "imageType": "DETAIL",
          "vendorPath": "http://image11.coupangcdn.com/image/product/image/vendoritem/2017/02/21/3000169918/34b79649-d625-4f49-a260-b78bf7a573a8.jpg"
        },
        {
          "imageOrder": 2,
          "imageType": "DETAIL",
          "vendorPath": "http://image11.coupangcdn.com/image/product/image/vendoritem/2018/06/28/3000169918/5716aa61-70bd-47cd-8f3d-f3d49e7f496d.jpg"
        }
      ],
      "notices": [
        {
          "noticeCategoryName": "화장품",
          "noticeCategoryDetailName": "용량(중량)",
          "content": "상세페이지 참조"
        },
        {
          "noticeCategoryName": "화장품",
          "noticeCategoryDetailName": "제품 주요 사양",
          "content": "상세페이지 참조"
        },
        {
          "noticeCategoryName": "화장품",
          "noticeCategoryDetailName": "사용기한 또는 개봉 후 사용기간",
          "content": "상세페이지 참조"
        },
        {
          "noticeCategoryName": "화장품",
          "noticeCategoryDetailName": "사용방법",
          "content": "상세페이지 참조"
        },
        {
          "noticeCategoryName": "화장품",
          "noticeCategoryDetailName": "제조업자 및 제조판매업자",
          "content": "상세페이지 참조"
        },
        {
          "noticeCategoryName": "화장품",
          "noticeCategoryDetailName": "제조국",
          "content": "상세페이지 참조"
        },
        {
          "noticeCategoryName": "화장품",
          "noticeCategoryDetailName": "화장품법에 따라 기재, 표시하여야 하는 모든 성분",
          "content": "상세페이지 참조"
        },
        {
          "noticeCategoryName": "화장품",
          "noticeCategoryDetailName": "식품의약품안전처 심사 필 유무",
          "content": "상세페이지 참조"
        },
        {
          "noticeCategoryName": "화장품",
          "noticeCategoryDetailName": "사용할 때 주의사항",
          "content": "상세페이지 참조"
        },
        {
          "noticeCategoryName": "화장품",
          "noticeCategoryDetailName": "품질보증기준",
          "content": "제품 이상 시 공정거래위원회 고시 소비자분쟁해결기준에 의거 보상합니다."
        },
        {
          "noticeCategoryName": "화장품",
          "noticeCategoryDetailName": "소비자상담관련 전화번호",
          "content": "상세페이지 참조"
        }
      ],
      "attributes": [
        {
          "attributeTypeName": "수량",
          "attributeValueName": "1개"
        },
        {
          "attributeTypeName": "개당 용량",
          "attributeValueName": "200ml"
        },
        {
          "attributeTypeName": "피부타입",
          "attributeValueName": "모든피부",
          "exposed": "NONE"
        },
        {
          "attributeTypeName": "피부고민",
          "attributeValueName": "모공",
          "exposed": "NONE"
        },
        {
          "attributeTypeName": "사용부위",
          "attributeValueName": "얼굴",
          "exposed": "NONE"
        }
      ],
      "contents": [
        {
          "contentsType": "TEXT",
          "contentDetails": [
            {
              "content": "<html><div></div><div><img src='http://image11.coupangcdn.com/image/product/content/vendorItem/2018/06/26/196713/738d905f-ed80-4fd8-ad21-ed87b195a19e.jpg' /><div></html>",
              "detailType": "TEXT"
            }
          ]
        }
      ],
      "offerCondition": "NEW",
      "offerDescription": ""
    },
    {
      "itemName": "200ml_2개",
      "originalPrice": 26000,
      "salePrice": 20000,
      "maximumBuyCount": 100,
      "maximumBuyForPerson": 0,
      "outboundShippingTimeDay": 2,
      "maximumBuyForPersonPeriod": 1,
      "unitCount": 1,
      "adultOnly": "EVERYONE",
      "taxType": "TAX",
      "parallelImported": "NOT_PARALLEL_IMPORTED",
      "overseasPurchased": "NOT_OVERSEAS_PURCHASED",
      "pccNeeded": "false",
      "externalVendorSku": "0001",
      "barcode": "",
      "emptyBarcode": true,
      "emptyBarcodeReason": "상품확인불가_바코드없음사유",
      "modelNo": "171717",
      "extraProperties": {
        "coupangSalePrice": 5000,
        "onlineSalePriceForBooks": 10000,
        "transactionType": "manufacturer",
        "businessType": "Beauty"
      },
      "certifications": [
        {
          "certificationType": "NOT_REQUIRED",
          "certificationCode": ""
        }
      ],
      "searchTags": [
        "검색어1",
        "검색어2"
      ],
      "images": [
        {
          "imageOrder": 0,
          "imageType": "REPRESENTATION",
          "vendorPath": "http://image11.coupangcdn.com/image/product/image/vendoritem/2018/06/26/3001519145/74100e2a-d1ad-4b50-9c78-840c12a3e10d.jpg"
        },
        {
          "imageOrder": 1,
          "imageType": "DETAIL",
          "vendorPath": "http://image11.coupangcdn.com/image/product/image/vendoritem/2017/02/21/3000169918/34b79649-d625-4f49-a260-b78bf7a573a8.jpg"
        },
        {
          "imageOrder": 2,
          "imageType": "DETAIL",
          "vendorPath": "http://image11.coupangcdn.com/image/product/image/vendoritem/2018/06/28/3000169918/5716aa61-70bd-47cd-8f3d-f3d49e7f496d.jpg"
        }
      ],
      "notices": [
        {
          "noticeCategoryName": "화장품",
          "noticeCategoryDetailName": "용량(중량)",
          "content": "상세페이지 참조"
        },
        {
          "noticeCategoryName": "화장품",
          "noticeCategoryDetailName": "제품 주요 사양",
          "content": "상세페이지 참조"
        },
        {
          "noticeCategoryName": "화장품",
          "noticeCategoryDetailName": "사용기한 또는 개봉 후 사용기간",
          "content": "상세페이지 참조"
        },
        {
          "noticeCategoryName": "화장품",
          "noticeCategoryDetailName": "사용방법",
          "content": "상세페이지 참조"
        },
        {
          "noticeCategoryName": "화장품",
          "noticeCategoryDetailName": "제조업자 및 제조판매업자",
          "content": "상세페이지 참조"
        },
        {
          "noticeCategoryName": "화장품",
          "noticeCategoryDetailName": "제조국",
          "content": "상세페이지 참조"
        },
        {
          "noticeCategoryName": "화장품",
          "noticeCategoryDetailName": "화장품법에 따라 기재, 표시하여야 하는 모든 성분",
          "content": "상세페이지 참조"
        },
        {
          "noticeCategoryName": "화장품",
          "noticeCategoryDetailName": "식품의약품안전처 심사 필 유무",
          "content": "상세페이지 참조"
        },
        {
          "noticeCategoryName": "화장품",
          "noticeCategoryDetailName": "사용할 때 주의사항",
          "content": "상세페이지 참조"
        },
        {
          "noticeCategoryName": "화장품",
          "noticeCategoryDetailName": "품질보증기준",
          "content": "상세페이지 참조"
        },
        {
          "noticeCategoryName": "화장품",
          "noticeCategoryDetailName": "소비자상담관련 전화번호",
          "content": "상세페이지 참조"
        }
      ],
      "attributes": [
        {
          "attributeTypeName": "수량",
          "attributeValueName": "2개"
        },
        {
          "attributeTypeName": "개당 용량",
          "attributeValueName": "200ml"
        },
        {
          "attributeTypeName": "피부타입",
          "attributeValueName": "모든피부",
          "exposed": "NONE"
        },
        {
          "attributeTypeName": "피부고민",
          "attributeValueName": "모공",
          "exposed": "NONE"
        },
        {
          "attributeTypeName": "사용부위",
          "attributeValueName": "얼굴",
          "exposed": "NONE"
        }
      ],
      "contents": [
        {
          "contentsType": "TEXT",
          "contentDetails": [
            {
              "content": "<html><div></div><div><img src='http://image11.coupangcdn.com/image/product/content/vendorItem/2018/06/26/196713/738d905f-ed80-4fd8-ad21-ed87b195a19e.jpg' /><div></html>",
              "detailType": "TEXT"
            }
          ]
        }
      ],
      "offerCondition": "NEW",
      "offerDescription": ""
    }
  ],
  "requiredDocuments": [
    {
      "templateName": "기타인증서류",
      "vendorDocumentPath": "http://image11.coupangcdn.com/image/product/content/vendorItem/2018/07/02/41579010/eebc0c30-8f35-4a51-8ffd-808953414dc1.jpg"
    }
  ],
  "extraInfoMessage": "",
  "manufacture": "아모레퍼시픽",
  "bundleInfo": {
    "bundleType": "SINGLE"
  }
}
```

## 2. Category Meta Info (카테고리 메타정보)
**Endpoint**: `GET /v2/providers/seller_api/apis/api/v1/marketplace/meta/categories/{categoryId}/attributes`

### Success Response (Meta Data)
```json
{
  "code": "SUCCESS",
  "message": "",
  "data": {
    "isAllowSingleItem": true,
    "attributes": [
      {
        "attributeTypeName": "수량",
        "dataType": "NUMBER",
        "basicUnit": "개",
        "usableUnits": [
          "개", "개입", "매", "매입", "팩", "입", "장", "병", "인용", "인분", "box", "Ea", "포", "캔", "박스", "스틱", "권", "set", "세트", "조", "캡슐", "통", "정", "봉", "마리", "롤", "p", "구", "피스", "회", "회분", "단계", "번", "프렛", "음판", "건반", "쪽", "단", "칸", "종", "분류", "현", "선", "공", "자리", "폭", "rpm", "색", "대"
        ],
        "required": "OPTIONAL",
        "groupNumber": "NONE",
        "exposed": "EXPOSED"
      },
      {
        "attributeTypeName": "자동차거치용품 거치대고정",
        "dataType": "STRING",
        "basicUnit": "없음",
        "usableUnits": [],
        "required": "OPTIONAL",
        "groupNumber": "NONE",
        "exposed": "NONE"
      },
      {
        "attributeTypeName": "헤드 회전 가능여부",
        "dataType": "STRING",
        "basicUnit": "없음",
        "usableUnits": [],
        "required": "OPTIONAL",
        "groupNumber": "NONE",
        "exposed": "NONE"
      },
      {
        "attributeTypeName": "각도조절 가능여부",
        "dataType": "STRING",
        "basicUnit": "없음",
        "usableUnits": [],
        "required": "OPTIONAL",
        "groupNumber": "NONE",
        "exposed": "NONE"
      }
    ],
    "noticeCategories": [
      {
        "noticeCategoryName": "자동차용품(자동차부품/기타 자동차용품)",
        "noticeCategoryDetailNames": [
          { "noticeCategoryDetailName": "품명 및 모델명", "required": "MANDATORY" },
          { "noticeCategoryDetailName": "출시년월", "required": "MANDATORY" },
          { "noticeCategoryDetailName": "KC 인증 필 유무", "required": "MANDATORY" },
          { "noticeCategoryDetailName": "제조자(수입자)", "required": "MANDATORY" },
          { "noticeCategoryDetailName": "제조국", "required": "MANDATORY" },
          { "noticeCategoryDetailName": "크기", "required": "MANDATORY" },
          { "noticeCategoryDetailName": "적용차종", "required": "MANDATORY" },
          { "noticeCategoryDetailName": "품질보증기준", "required": "MANDATORY" },
          { "noticeCategoryDetailName": "A/S 책임자와 전화번호", "required": "MANDATORY" }
        ]
      },
      {
        "noticeCategoryName": "기타 재화",
        "noticeCategoryDetailNames": [
          { "noticeCategoryDetailName": "품명 및 모델명", "required": "MANDATORY" },
          { "noticeCategoryDetailName": "인증사항", "required": "MANDATORY" },
          { "noticeCategoryDetailName": "제조국(원산지)", "required": "MANDATORY" },
          { "noticeCategoryDetailName": "제조자(수입자)", "required": "MANDATORY" },
          { "noticeCategoryDetailName": "소비자상담 관련 전화번호", "required": "MANDATORY" }
        ]
      }
    ],
    "requiredDocumentNames": [
      { "templateName": "기타인증서류", "required": "OPTIONAL" },
      { "templateName": "수입신고필증(병행수입 선택시)", "required": "MANDATORY_PARALLEL_IMPORTED" },
      { "templateName": "인보이스영수증(해외구매대행 선택시)", "required": "MANDATORY_OVERSEAS_PURCHASED" }
    ],
    "certifications": [
      { "certificationType": "NOT_REQUIRED", "name": "인증대상아님", "dataType": "NONE", "required": "OPTIONAL" },
      { "certificationType": "PRESENTED_IN_DETAIL_PAGE", "name": "상세설명에 표시", "dataType": "NONE", "required": "OPTIONAL" },
      { "certificationType": "KC_KID_CERTIFICATION", "name": "KC인증 어린이제품 안전인증", "dataType": "CODE", "required": "RECOMMEND" },
      { "certificationType": "KC_KID_CONFIRM", "name": "KC인증 어린이제품 안전확인", "dataType": "CODE", "required": "OPTIONAL" },
      { "certificationType": "KC_KID_PROVIDER", "name": "KC인증 어린이제품 공급자적합성확인", "dataType": "NONE", "required": "OPTIONAL" },
      { "certificationType": "KC_ELECTRONICS_CERTIFICATION", "name": "KC인증 전기용품 안전인증", "dataType": "CODE", "required": "OPTIONAL" },
      { "certificationType": "KC_ELECTRONICS_CONFIRM", "name": "KC인증 전기용품 안전확인", "dataType": "CODE", "required": "OPTIONAL" },
      { "certificationType": "KC_ELECTRONICS_PROVIDER", "name": "KC인증 전기용품 공급자적합성확인", "dataType": "NONE", "required": "OPTIONAL" },
      { "certificationType": "KC_HOUSEHOLD_CERTIFICATION", "name": "KC인증 생활용품 안전인증", "dataType": "CODE", "required": "RECOMMEND" },
      { "certificationType": "KC_HOUSEHOLD_CONFIRM", "name": "KC인증 생활용품 자율안전확인", "dataType": "CODE", "required": "RECOMMEND" },
      { "certificationType": "KC_HOUSEHOLD_QUALITY", "name": "KC인증 생활용품 안전품질표시", "dataType": "NONE", "required": "RECOMMEND" },
      { "certificationType": "KC_HOUSEHOLD_PACKAGING", "name": "KC인증 생활용품 어린이보호포장", "dataType": "NONE", "required": "RECOMMEND" },
      { "certificationType": "COMMUNICATION_EQUIPMENT", "name": "방송통신기자재 적합성 평가 대상", "dataType": "CODE", "required": "RECOMMEND" }
    ],
    "allowedOfferConditions": [ "NEW", "REFURBISHED", "USED_BEST", "USED_GOOD", "USED_NORMAL" ]
  }
}
```

## 3. Product Inquiry (상품 조회)
**Endpoint**: `GET /v2/providers/seller_api/apis/api/v1/marketplace/seller-products/{sellerProductId}`

### Success Response Example
```json
{
  "code": "SUCCESS",
  "message": "",
  "data": {
    "sellerProductId": 123459542,
    "sellerProductName": "test_클렌징오일_관리용_상품명",
    "displayCategoryCode": 56137,
    "categoryId": 1617,
    "productId": 131023672,
    "vendorId": "A0001235",
    "mdId": "NLUP",
    "mdName": "dummy",
    "saleStartedAt": "2019-01-09T18:41:14",
    "saleEndedAt": "2099-01-01T23:59:59",
    "displayProductName": "해피바스 솝베리 클렌징 오일",
    "brand": "해피바스",
    "generalProductName": "솝베리 클렌징 오일",
    "productGroup": "클렌징 오일",
    "statusName": "승인완료",
    "deliveryMethod": "VENDOR_DIRECT",
    "deliveryCompanyCode": "KDEXP",
    "deliveryChargeType": "FREE",
    "deliveryCharge": 0,
    "freeShipOverAmount": 0,
    "deliveryChargeOnReturn": 2500,
    "deliverySurcharge": 0,
    "remoteAreaDeliverable": "N",
    "bundlePackingDelivery": 0,
    "unionDeliveryType": "UNION_DELIVERY",
    "returnCenterCode": "1234274592",
    "returnChargeName": "반품지_1",
    "companyContactNumber": "02-1234-678",
    "returnZipCode": "06168",
    "returnAddress": "서울특별시 강남구 삼성동",
    "returnAddressDetail": "1-23 19층",
    "returnCharge": 2500,
    "outboundShippingPlaceCode": 74010,
    "vendorUserId": "wing_loginId_123",
    "requested": false,
    "items": [
      {
        "offerCondition": "NEW",
        "offerDescription": null,
        "sellerProductItemId": 1271845812,
        "vendorItemId": 4279191312,
        "itemId": 362266710,
        "itemName": "200ml_1개_(변경될수있음)",
        "originalPrice": 0,
        "salePrice": 1280960,
        "supplyPrice": 1111873,
        "maximumBuyCount": 1,
        "maximumBuyForPerson": 0,
        "outboundShippingTimeDay": 2,
        "maximumBuyForPersonPeriod": 1,
        "unitCount": 1,
        "adultOnly": "EVERYONE",
        "taxType": "TAX",
        "parallelImported": "NOT_PARALLEL_IMPORTED",
        "overseasPurchased": "NOT_OVERSEAS_PURCHASED",
        "externalVendorSku": "0001",
        "pccNeeded": false,
        "bestPriceGuaranteed3P": false,
        "emptyBarcode": true,
        "emptyBarcodeReason": "상품확인불가_바코드없음사유",
        "barcode": null,
        "saleAgentCommission": 9,
        "modelNo": "171717",
        "images": [
          { "imageOrder": 0, "imageType": "REPRESENTATION", "cdnPath": "vendor_inventory/images/2019/01/09/18/9/3c1cee6d-9ab1-454a-8742-de94215cab1b.jpg", "vendorPath": "151009021007000006.jpg" },
          { "imageOrder": 1, "imageType": "DETAIL", "cdnPath": "vendor_inventory/images/2019/01/09/18/4/b43651a8-974e-4965-a650-9238ea1ecc15.jpg", "vendorPath": "plg27673_0000004440.jpg" }
        ],
        "notices": [
          { "noticeCategoryName": "화장품", "noticeCategoryDetailName": "용량(중량)", "content": "상세페이지 참조" },
          { "noticeCategoryName": "화장품", "noticeCategoryDetailName": "제품 주요 사양", "content": "상세페이지 참조" },
          { "noticeCategoryName": "화장품", "noticeCategoryDetailName": "사용기한 또는 개봉 후 사용기간", "content": "상세페이지 참조" },
          { "noticeCategoryName": "화장품", "noticeCategoryDetailName": "사용방법", "content": "상세페이지 참조" },
          { "noticeCategoryName": "화장품", "noticeCategoryDetailName": "제조업자 및 제조판매업자", "content": "상세페이지 참조" },
          { "noticeCategoryName": "화장품", "noticeCategoryDetailName": "제조국", "content": "상세페이지 참조" },
          { "noticeCategoryName": "화장품", "noticeCategoryDetailName": "화장품법에 따라 기재, 표시하여야 하는 모든 성분", "content": "상세페이지 참조" },
          { "noticeCategoryName": "화장품", "noticeCategoryDetailName": "식품의약품안전처 심사 필 유무", "content": "상세페이지 참조" },
          { "noticeCategoryName": "화장품", "noticeCategoryDetailName": "사용할 때 주의사항", "content": "상세페이지 참조" },
          { "noticeCategoryName": "화장품", "noticeCategoryDetailName": "품질보증기준", "content": "제품 이상 시 공정거래위원회 고시 소비자분쟁해결기준에 의거 보상합니다." },
          { "noticeCategoryName": "화장품", "noticeCategoryDetailName": "소비자상담관련 전화번호", "content": "상세페이지 참조" }
        ],
        "attributes": [
          { "attributeTypeName": "피부타입", "attributeValueName": "", "exposed": "NONE", "editable": true },
          { "attributeTypeName": "피부고민", "attributeValueName": "", "exposed": "NONE", "editable": true },
          { "attributeTypeName": "수량", "attributeValueName": "1개", "exposed": "EXPOSED", "editable": true },
          { "attributeTypeName": "개당 중량", "attributeValueName": "", "exposed": "EXPOSED", "editable": true },
          { "attributeTypeName": "사용 부위", "attributeValueName": "", "exposed": "NONE", "editable": true },
          { "attributeTypeName": "개당 용량", "attributeValueName": "200ml", "exposed": "EXPOSED", "editable": true }
        ],
        "contents": [
          {
            "contentsType": "TEXT",
            "contentDetails": [
              { "content": "<div></div> <div> <img src=\"http://img1a.coupangcdn.com/image/vendor_inventory/images/2019/01/09/18/7/0338f9a4-d6cb-4713-9948-5f49c4a61f27.jpeg\"> <div></div> </div>", "detailType": "TEXT" }
            ]
          }
        ],
        "certifications": [ { "certificationType": "NOT_REQUIRED", "certificationCode": "" } ],
        "extraProperties": { "transactionType": "manufacturer", "onlineSalePriceForBooks": "10000", "coupangSalePrice": "5000", "businessType": "Beauty" },
        "searchTags": [ "검색어1", "검색어2" ]
      }
    ],
    "requiredDocuments": [],
    "extraInfoMessage": null,
    "manufacture": "제조사_테스트",
    "bundleInfo": { "bundleType": "SINGLE" }
  }
}
```

---
## 관련 문서
- [[📊 대시보드|📊 대시보드]]
- [[Projects/셀픽스-쿠팡-파이프라인|🛒 쿠팡 파이프라인]]
- [[Memory/운영-규칙|⚙️ 운영 규칙]]
- [[Memory/장기기억|🧠 장기기억]]
- [[History/지금까지-한-일|📅 전체 타임라인]]
