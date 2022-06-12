## Document

### ■ 이체 내역 조회 Transfer History - POST
--------------------------------------------
카카오뱅크에서 개설된 계좌의 이체내역을 조회한다.

|HTTP|
|-|
|POST ~/transferHistory|

#### ● 요청 헤더
|key|value|
|-|-|
|Bearer|Token|

#### ● 요청 본문
|Name|Required|Type|Description|
|-|-|-|-|
|accountIdx  |true|int|조회할 계좌의 idx입니다.|
|periodStart |false|string|조회 기간 시작일입니다.|
|periodEnd   |false|string|조회 기간 종료일입니다.|
|status      |false|string|조회할 이체 상태입니다.|

#### ● 응답
|Name|Type|Description|
|-|-|-|
|200 OK|AccountHistory|이체내역을 검색했습니다.|
|Other Status Codes||내부 오류처리에 따릅니다.|

*AccountHistory*
|Name|Type|Description|
|-|-|-|
transferIdx|   int|이체 내역의 index입니다.|
type|          string|이체 유형입니다.|
receiverName|  string|수신인 이름입니다.|
status|        string|이체 상태입니다.|
amount|        int|이체 금액입니다.|
registDate|    string|이체 신청일입니다.|
endDate|       string|이체 처리일입니다.|

#### ● 응답 예시
```
HTTP/1.1 200 OK
Content-type: application/json;charset=UTF-8
{
    "data": [
        {
            "transferIdx": "1234567",
            "type": "간편이체",
            "receiverName": "홍길동",
            "status": "입금대기중",
            "amount": "10000",
            "registDate": "2021.01.01",
            "endDate": ""
        },
        ...
    ]
}
```



&nbsp;
### ■ 간편이체 계좌목록 조회 Easy Transferable Accounts - POST
--------------------------------------------
간편이체를 이용할 수 있는 계좌 목록을 조회합니다.

|HTTP|
|-|
|POST ~/getEasyTransferableAccountList|

#### ● 요청 헤더

|key|value|
|-|-|
|Bearer|Token|

#### ● 요청 본문

#### ● 응답
|Name|Type|Description|
|-|-|-|
|200 OK|Account|계좌 목록을 검색했습니다.|
|Other Status Codes||내부 오류처리에 따릅니다.|

*Account*
|Name|Type|Description|
|-|-|-|
accountIdx      |int|계좌의 idx입니다.|
accountNumber   |string|계좌의 계좌번호입니다.|
accountName     |string|계좌의 별명입니다.|
balance         |string|계좌의 잔액입니다.|

#### ● 응답 예시
```
HTTP/1.1 200 OK
Content-type: application/json;charset=UTF-8
{
    "data": [
        {
            "accountIdx": "1234567",
            "accountNumber": "3333-01-123456",
            "accountName": "비상금",
            "balance": "10000"
        },
        ...
    ]
}
```

&nbsp;
### ■ 간편이체 송금 Easy Transfer Request - POST
--------------------------------------------
카카오톡 친구에게 송금합니다.

|HTTP|
|-|
|POST ~/easyTransfer|

#### ● 요청 헤더

|key|value|
|-|-|
|Bearer|Token|

#### ● 요청 본문
|Name|Required|Type|Description|
|-|-|-|-|
|receiverUUID      |true|string|카카오톡 친구목록에서 가져온 친구 uuid입니다.|
|receiverNickName  |true|string|카카오톡 친구목록에서 가져온 친구 별명입니다.|
|receiverName      |true|string|송금할 친구의 실명입니다.|
|withdrawAccountIdx|true|long|송금할 계좌의 index입니다.|
|amount            |true|int|송금할 금액입니다.|
|memo              |false|string|이체내역에 기록될 메모입니다.|
|message           |false|string|카카오톡 메시지 내용입니다.|
|templateObjectType|false|string|카카오톡 메시지의 템플릿 index입니다.|

##### ● 응답
|Name|Type|Description|
|-|-|-|
|200 OK|KakaotalkWithdrawal|카카오톡 메시지 버튼 링크에 포함되는 정보입니다.|
|Other Status Codes||내부 오류처리에 따릅니다.|

*KakaotalkWithdrawal*
|Name|Type|Description|
|-|-|-|
|tx_dt              |string|확인불가|
|tx_seqno           |string|확인불가|
|inbn_trsf_tx_kncd  |string|확인불가|

#### ● 응답 예시
```
HTTP/1.1 200 OK
Content-type: application/json;charset=UTF-8
{
    "tx_dt": "-----",
    "tx_seqno": "-----",
    "inbn_trsf_tx_kncd": "-----"
}
```


&nbsp;
### ■ 간편이체 입금 Kakaotalk Withdrawal - GET
--------------------------------------------
간편송금회사계정으로부터 수신인의 계좌로 입금을 진행합니다.

|HTTP|
|-|
|GET ~/kakaotalkWithdrawal|

#### ● 요청 파라미터
|Name|Required|Type|Description|
|-|-|-|-|
|tx_dt            |true|string|확인불가|
|tx_seqno         |true|string|확인불가|
|inbn_trsf_tx_kncd|true|string|확인불가|

#### ● 응답
|Name|Type|Description|
|-|-|-|
|200 OK|Object|입금 성공 정보입니다.|
|Other Status Codes||내부 오류처리에 따릅니다.|

#### ● 응답 예시
```
HTTP/1.1 200 OK
Content-type: application/json;charset=UTF-8
{
    ...
}
```