---
title: ?? Excel JavaScript API ???? Excel ?????
description: ''
ms.date: 01/24/2017
ms.openlocfilehash: 5eb78484917cc3d4700c95d69fb1e83b6836d1cc
ms.sourcegitcommit: c72c35e8389c47a795afbac1b2bcf98c8e216d82
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/23/2018
---
# <a name="call-built-in-excel-worksheet-functions"></a>???? Excel ?????

????????? Excel JavaScript API ???? Excel ??????? `VLOOKUP` ? `SUM`?? ?????????? Excel JavaScript API ????? Excel ???????????

> [!NOTE]
> ???????? Excel JavaScript API ? Excel ???*?????*????[? Excel ????????](custom-functions-overview.md)?

## <a name="calling-a-worksheet-function"></a>???????

?????????????????????? `sampleFunction()` ???????????????????????????????? ???????? **FunctionResult** ??? **value** ???????????? ??????????? **FunctionResult** ??? **value** ???? `load` ??????????? ??????????????????? 

```js
var functionResult = context.workbook.functions.sampleFunction(); 
functionResult.load('value');
return context.sync()
    .then(function () {
        console.log('Result of the function: ' + functionResult.value);
    });
```

> [!TIP]
> ?????? Excel JavaScript API ??????????????[????????](#supported-worksheet-functions)???

## <a name="sample-data"></a>????

????? Excel ?????????????????????????? ??????????????????????????? ????????????????????????????

![????????? 11 ??12 ?? 1 ?????? Excel ????](../images/worksheet-functions-chaining-results.jpg)

## <a name="example-1-single-function"></a>?? 1????

???????????????? `VLOOKUP` ?????? 11 ????????

```js
Excel.run(function (context) {
    var range = context.workbook.worksheets.getItem("Sheet1").getRange("A1:D4");
    var unitSoldInNov = context.workbook.functions.vlookup("Wrench", range, 2, false);
    unitSoldInNov.load('value');

    return context.sync()
        .then(function () {
            console.log(' Number of wrenches sold in November = ' + unitSoldInNov.value);
        });
}).catch(errorHandlerFunction);
```

## <a name="example-2-nested-functions"></a>?? 2?????

???????????????? `VLOOKUP` ???????? 11 ?? 12 ????????????? `SUM` ?????????????????? 

?????????????????????????????????????????????????? `sumOfTwoLookups`??? `load` ????? ????????????????????? `VLOOKUP` ???????????????????????

```js
Excel.run(function (context) {
    var range = context.workbook.worksheets.getItem("Sheet1").getRange("A1:D4");
    var sumOfTwoLookups = context.workbook.functions.sum(
        context.workbook.functions.vlookup("Wrench", range, 2, false), 
        context.workbook.functions.vlookup("Wrench", range, 3, false)
    );
    sumOfTwoLookups.load('value');

    return context.sync()
        .then(function () {
            console.log(' Number of wrenches sold in November and December = ' + sumOfTwoLookups.value);
        });
}).catch(errorHandlerFunction);
```

## <a name="supported-worksheet-functions"></a>????????

???? Excel JavaScript API ?????? Excel ??????

| ?? | ???? | ?? |
|:---------------|:-------------|:-----------|
| <a href="https://support.office.com/en-us/article/ABS-function-3420200f-5628-4e8c-99da-c99d7c87713c" target="_blank">ABS ??</a> | FunctionResult | ???????? |
| <a href="https://support.office.com/en-us/article/ACCRINT-function-fe45d089-6722-4fb3-9379-e1f911d8dc74" target="_blank">ACCRINT ??</a> | FunctionResult | ???????????????? |
| <a href="https://support.office.com/en-us/article/ACCRINTM-function-f62f01f9-5754-4cc4-805b-0e70199328a7" target="_blank">ACCRINTM ??</a> | FunctionResult | ?????????????????? |
| <a href="https://support.office.com/en-us/article/ACOS-function-cb73173f-d089-4582-afa1-76e5524b5d5b" target="_blank">ACOS ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/ACOSH-function-e3992cc1-103f-4e72-9f04-624b9ef5ebfe" target="_blank">ACOSH ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/ACOT-function-dc7e5008-fe6b-402e-bdd6-2eea8383d905" target="_blank">ACOT ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/ACOTH-function-cc49480f-f684-4171-9fc5-73e4e852300f" target="_blank">ACOTH ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/AMORDEGRC-function-a14d0ca1-64a4-42eb-9b3d-b0dededf9e51" target="_blank">AMORDEGRC ??</a> | FunctionResult | ????????????????????? |
| <a href="https://support.office.com/en-us/article/AMORLINC-function-7d417b45-f7f5-4dba-a0a5-3451a81079a8" target="_blank">AMORLINC ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/AND-function-5f19b2e8-e1df-4408-897a-ce285a19e9d9" target="_blank">AND ??</a> | FunctionResult | ???????? true???`TRUE` |
| <a href="https://support.office.com/en-us/article/ARABIC-function-9a8da418-c17b-4ef9-a657-9370a30a674f" target="_blank">ARABIC ??</a> | FunctionResult | ????????????? |
| <a href="https://support.office.com/en-us/article/AREAS-function-8392ba32-7a41-43b3-96b0-3695d2ec6152" target="_blank">AREAS ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/ASC-function-0b6abf1c-c663-4004-a964-ebc00b723266" target="_blank">ASC ??</a> | FunctionResult | ????????????????????????????????? |
| <a href="https://support.office.com/en-us/article/ASIN-function-81fb95e5-6d6f-48c4-bc45-58f955c6d347" target="_blank">ASIN ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/ASINH-function-4e00475a-067a-43cf-926a-765b0249717c" target="_blank">ASINH ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/ATAN-function-50746fa8-630a-406b-81d0-4a2aed395543" target="_blank">ATAN ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/ATAN2-function-c04592ab-b9e3-4908-b428-c96b3a565033" target="_blank">ATAN2 ??</a> | FunctionResult | ??? x ??? y ??????? |
| <a href="https://support.office.com/en-us/article/ATANH-function-3cd65768-0de7-4f1d-b312-d01c8c930d90" target="_blank">ATANH ??</a> | FunctionResult | ????????????? |
| <a href="https://support.office.com/en-us/article/AVEDEV-function-58fe8d65-2a84-4dc7-8052-f3f87b5c6639" target="_blank">AVEDEV ??</a> | FunctionResult | ??????????????????????? |
| <a href="https://support.office.com/en-us/article/AVERAGE-function-047bac88-d466-426c-a32b-8f33eb960cf6" target="_blank">AVERAGE ??</a> | FunctionResult | ????????? |
| <a href="https://support.office.com/en-us/article/AVERAGEA-function-f5f84098-d453-4f4c-bbba-3d2c66356091" target="_blank">AVERAGEA ??</a> | FunctionResult | ????????????????????? |
| <a href="https://support.office.com/en-us/article/AVERAGEIF-function-faec8e2e-0dec-4308-af69-f5576d8ac642" target="_blank">AVERAGEIF ??</a> | FunctionResult | ???????????????????????????? |
| <a href="https://support.office.com/en-us/article/AVERAGEIFS-function-48910c45-1fc0-4389-a028-f7c5c3001690" target="_blank">AVERAGEIFS ??</a> | FunctionResult | ????????????????????????? |
| <a href="https://support.office.com/en-us/article/BAHTTEXT-function-5ba4d0b4-abd3-4325-8d22-7a92d59aab9c" target="_blank">BAHTTEXT ??</a> | FunctionResult | ?? ???????????????? |
| <a href="https://support.office.com/en-us/article/BASE-function-2ef61411-aee9-4f29-a811-1c42456c6342" target="_blank">BASE ??</a> | FunctionResult | ??????????????????? |
| <a href="https://support.office.com/en-us/article/BESSELI-function-8d33855c-9a8d-444b-98e0-852267b1c0df" target="_blank">BESSELI ??</a> | FunctionResult | ?????????? In(x) |
| <a href="https://support.office.com/en-us/article/BESSELJ-function-839cb181-48de-408b-9d80-bd02982d94f7" target="_blank">BESSELJ ??</a> | FunctionResult | ??????? Jn(x) |
| <a href="https://support.office.com/en-us/article/BESSELK-function-606d11bc-06d3-4d53-9ecb-2803e2b90b70" target="_blank">BESSELK ??</a> | FunctionResult | ?????????? Kn(x) |
| <a href="https://support.office.com/en-us/article/BESSELY-function-f3a356b3-da89-42c3-8974-2da54d6353a2" target="_blank">BESSELY ??</a> | FunctionResult | ??????? Yn(x) |
| <a href="https://support.office.com/en-us/article/BETADIST-function-11188c9c-780a-42c7-ba43-9ecb5a878d31" target="_blank">BETA.DIST ??</a> | FunctionResult | ?? beta ?????? |
| <a href="https://support.office.com/en-us/article/BETAINV-function-e84cb8aa-8df0-4cf6-9892-83a341d252eb" target="_blank">BETA.INV ??</a> | FunctionResult | ????? beta ???????????? |
| <a href="https://support.office.com/en-us/article/BIN2DEC-function-63905b57-b3a0-453d-99f4-647bb519cd6c" target="_blank">BIN2DEC ??</a> | FunctionResult | ??????????? |
| <a href="https://support.office.com/en-us/article/BIN2HEX-function-0375e507-f5e5-4077-9af8-28d84f9f41cc" target="_blank">BIN2HEX ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/BIN2OCT-function-0a4e01ba-ac8d-4158-9b29-16c25c4c23fd" target="_blank">BIN2OCT ??</a> | FunctionResult | ??????????? |
| <a href="https://support.office.com/en-us/article/BINOMDIST-function-c5ae37b6-f39c-4be2-94c2-509a1480770c" target="_blank">BINOM.DIST ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/BINOMDISTRANGE-function-17331329-74c7-4053-bb4c-6653a7421595" target="_blank">BINOM.DIST.RANGE ??</a> | FunctionResult | ????????????????? |
| <a href="https://support.office.com/en-us/article/BINOMINV-function-80a0370c-ada6-49b4-83e7-05a91ba77ac9" target="_blank">BINOM.INV ??</a> | FunctionResult | ??????????????????????????????????? |
| <a href="https://support.office.com/en-us/article/BITAND-function-8a2be3d7-91c3-4b48-9517-64548008563a" target="_blank">BITAND ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/BITLSHIFT-function-c55bb27e-cacd-4c7c-b258-d80861a03c9c" target="_blank">BITLSHIFT ??</a> | FunctionResult | ???? shift_amount ?????????? |
| <a href="https://support.office.com/en-us/article/BITOR-function-f6ead5c8-5b98-4c9e-9053-8ad5234919b2" target="_blank">BITOR ??</a> | FunctionResult | ?? 2 ????????? |
| <a href="https://support.office.com/en-us/article/BITRSHIFT-function-274d6996-f42c-4743-abdb-4ff95351222c" target="_blank">BITRSHIFT ??</a> | FunctionResult | ???? shift_amount ?????????? |
| <a href="https://support.office.com/en-us/article/BITXOR-function-c81306a1-03f9-4e89-85ac-b86c3cba10e4" target="_blank">BITXOR ??</a> | FunctionResult | ?????????????? |
| <a href="https://support.office.com/en-us/article/CEILINGMATH-function-80f95d2f-b499-4eee-9f16-f795a8e306c8" target="_blank">CEILING.MATH ??</a> | FunctionResult | ???????????????????????? |
| <a href="https://support.office.com/en-us/article/CEILINGPRECISE-function-f366a774-527a-4c92-ba49-af0a196e66cb" target="_blank">CEILING.PRECISE ??</a> | FunctionResult | ????????????????????????????????????????????? |
| <a href="https://support.office.com/en-us/article/CHAR-function-bbd249c8-b36e-4a91-8017-1c133f9b837a" target="_blank">CHAR ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/CHISQDIST-function-8486b05e-5c05-4942-a9ea-f6b341518732" target="_blank">CHISQ.DIST ??</a> | FunctionResult | ???? beta ????????? |
| <a href="https://support.office.com/en-us/article/CHISQDISTRT-function-dc4832e8-ed2b-49ae-8d7c-b28d5804c0f2" target="_blank">CHISQ.DIST.RT ??</a> | FunctionResult | ?? ?2 ??????? |
| <a href="https://support.office.com/en-us/article/CHISQINV-function-400db556-62b3-472d-80b3-254723e7092f" target="_blank">CHISQ.INV ??</a> | FunctionResult | ???? beta ????????? |
| <a href="https://support.office.com/en-us/article/CHISQINVRT-function-435b5ed8-98d5-4da6-823f-293e2cbc94fe" target="_blank">CHISQ.INV.RT ??</a> | FunctionResult | ?? ?2 ??????????? |
| <a href="https://support.office.com/en-us/article/CHOOSE-function-fc5c184f-cb62-4ec7-a46e-38653b98f5bc" target="_blank">CHOOSE ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/CLEAN-function-26f3d7c5-475f-4a9c-90e5-4b8ba987ba41" target="_blank">CLEAN ??</a> | FunctionResult | ????????????? |
| <a href="https://support.office.com/en-us/article/CODE-function-c32b692b-2ed0-4a04-bdd9-75640144b928" target="_blank">CODE ??</a> | FunctionResult | ?????????????????? |
| <a href="https://support.office.com/en-us/article/COLUMNS-function-4e8e7b4e-e603-43e8-b177-956088fa48ca" target="_blank">COLUMNS ??</a> | FunctionResult | ???????? |
| <a href="https://support.office.com/en-us/article/COMBIN-function-12a3f276-0a21-423a-8de6-06990aaf638a" target="_blank">COMBIN ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/COMBINA-function-efb49eaa-4f4c-4cd2-8179-0ddfcf9d035d" target="_blank">COMBINA ??</a> | FunctionResult | ????????????????? |
| <a href="https://support.office.com/en-us/article/COMPLEX-function-f0b8f3a9-51cc-4d6d-86fb-3a9362fa4128" target="_blank">COMPLEX ??</a> | FunctionResult | ??????????????? |
| <a href="https://support.office.com/en-us/article/CONCATENATE-function-8f8ae884-2ca8-4f7a-b093-75d702bea31d" target="_blank">CONCATENATE ??</a> | FunctionResult | ?????????????? |
| <a href="https://support.office.com/en-us/article/CONFIDENCENORM-function-7cec58a6-85bb-488d-91c3-63828d4fbfd4" target="_blank">CONFIDENCE.NORM ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/CONFIDENCET-function-e8eca395-6c3a-4ba9-9003-79ccc61d3c53" target="_blank">CONFIDENCE.T ??</a> | FunctionResult | ???? t ?????????????? |
| <a href="https://support.office.com/en-us/article/CONVERT-function-d785bef1-808e-4aac-bdcd-666c810f9af2" target="_blank">CONVERT ??</a> | FunctionResult | ???????????????????? |
| <a href="https://support.office.com/en-us/article/COS-function-0fb808a5-95d6-4553-8148-22aebdce5f05" target="_blank">COS ??</a> | FunctionResult | ????????? |
| <a href="https://support.office.com/en-us/article/COSH-function-e460d426-c471-43e8-9540-a57ff3b70555" target="_blank">COSH ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/COT-function-c446f34d-6fe4-40dc-84f8-cf59e5f5e31a" target="_blank">COT ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/COTH-function-2e0b4cb6-0ba0-403e-aed4-deaa71b49df5" target="_blank">COTH ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/COUNT-function-a59cd7fc-b623-4d93-87a4-d23bf411294c" target="_blank">COUNT ??</a> | FunctionResult | ??????????? |
| <a href="https://support.office.com/en-us/article/COUNTA-function-7dc98875-d5c1-46f1-9a82-53f3219e2509" target="_blank">COUNTA ??</a> | FunctionResult | ??????????? |
| <a href="https://support.office.com/en-us/article/COUNTBLANK-function-6a92d772-675c-4bee-b346-24af6bd3ac22" target="_blank">COUNTBLANK ??</a> | FunctionResult | ??????????????? |
| <a href="https://support.office.com/en-us/article/COUNTIF-function-e0de10c6-f885-4e71-abb4-1f464816df34" target="_blank">COUNTIF ??</a> | FunctionResult | ??????????????????? |
| <a href="https://support.office.com/en-us/article/COUNTIFS-function-dda3dc6e-f74e-4aee-88bc-aa8c2a866842" target="_blank">COUNTIFS ??</a> | FunctionResult | ??????????????????? |
| <a href="https://support.office.com/en-us/article/COUPDAYBS-function-eb9a8dfb-2fb2-4c61-8e5d-690b320cf872" target="_blank">COUPDAYBS ??</a> | FunctionResult | ????????????????? |
| <a href="https://support.office.com/en-us/article/COUPDAYS-function-cc64380b-315b-4e7b-950c-b30b0a76f671" target="_blank">COUPDAYS ??</a> | FunctionResult | ?????????????? |
| <a href="https://support.office.com/en-us/article/COUPDAYSNC-function-5ab3f0b2-029f-4a8b-bb65-47d525eea547" target="_blank">COUPDAYSNC ??</a> | FunctionResult | ??????????????????? |
| <a href="https://support.office.com/en-us/article/COUPNCD-function-fd962fef-506b-4d9d-8590-16df5393691f" target="_blank">COUPNCD ??</a> | FunctionResult | ?????????????? |
| <a href="https://support.office.com/en-us/article/COUPNUM-function-a90af57b-de53-4969-9c99-dd6139db2522" target="_blank">COUPNUM ??</a> | FunctionResult | ?????????????????? |
| <a href="https://support.office.com/en-us/article/COUPPCD-function-2eb50473-6ee9-4052-a206-77a9a385d5b3" target="_blank">COUPPCD ??</a> | FunctionResult | ?????????????? |
| <a href="https://support.office.com/en-us/article/CSC-function-07379361-219a-4398-8675-07ddc4f135c1" target="_blank">CSC ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/CSCH-function-f58f2c22-eb75-4dd6-84f4-a503527f8eeb" target="_blank">CSCH ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/CUMIPMT-function-61067bb0-9016-427d-b95b-1a752af0e606" target="_blank">CUMIPMT ??</a> | FunctionResult | ??????????????????? |
| <a href="https://support.office.com/en-us/article/CUMPRINC-function-94a4516d-bd65-41a1-bc16-053a6af4c04d" target="_blank">CUMPRINC ??</a> | FunctionResult | ??????????????????? |
| <a href="https://support.office.com/en-us/article/DATE-function-e36c0c8c-4104-49da-ab83-82328b832349" target="_blank">DATE ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/DATEVALUE-function-df8b07d4-7761-4a93-bc33-b7471bbff252" target="_blank">DATEVALUE ??</a> | FunctionResult | ??????????????? |
| <a href="https://support.office.com/en-us/article/DAVERAGE-function-a6a2d5ac-4b4b-48cd-a1d8-7b37834e5aee" target="_blank">DAVERAGE ??</a> | FunctionResult | ????????????? |
| <a href="https://support.office.com/en-us/article/DAY-function-8a7d1cbb-6c7d-4ba1-8aea-25c134d03101" target="_blank">DAY ??</a> | FunctionResult | ?????????????? |
| <a href="https://support.office.com/en-us/article/DAYS-function-57740535-d549-4395-8728-0f07bff0b9df" target="_blank">DAYS ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/DAYS360-function-b9a509fd-49ef-407e-94df-0cbda5718c2a" target="_blank">DAYS360 ??</a> | FunctionResult | ??? 360 ????????????? |
| <a href="https://support.office.com/en-us/article/DB-function-354e7d28-5f93-4ff1-8a52-eb4ee549d9d7" target="_blank">DB ??</a> | FunctionResult | ???????????????????????? |
| <a href="https://support.office.com/en-us/article/DBCS-function-a4025e73-63d2-4958-9423-21a24794c9e5" target="_blank">DBCS ??</a> | FunctionResult | ????????????????????????????????? |
| <a href="https://support.office.com/en-us/article/DCOUNT-function-c1fc7b93-fb0d-4d8d-97db-8d5f076eaeb1" target="_blank">DCOUNT ??</a> | FunctionResult | ???????????????? |
| <a href="https://support.office.com/en-us/article/DCOUNTA-function-00232a6d-5a66-4a01-a25b-c1653fda1244" target="_blank">DCOUNTA ??</a> | FunctionResult | ??????????????? |
| <a href="https://support.office.com/en-us/article/DDB-function-519a7a37-8772-4c96-85c0-ed2c209717a5" target="_blank">DDB ??</a> | FunctionResult | ???????????????????????????????? |
| <a href="https://support.office.com/en-us/article/DEC2BIN-function-0f63dd0e-5d1a-42d8-b511-5bf5c6d43838" target="_blank">DEC2BIN ??</a> | FunctionResult | ??????????? |
| <a href="https://support.office.com/en-us/article/DEC2HEX-function-6344ee8b-b6b5-4c6a-a672-f64666704619" target="_blank">DEC2HEX ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/DEC2OCT-function-c9d835ca-20b7-40c4-8a9e-d3be351ce00f" target="_blank">DEC2OCT ??</a> | FunctionResult | ??????????? |
| <a href="https://support.office.com/en-us/article/DECIMAL-function-ee554665-6176-46ef-82de-0a283658da2e" target="_blank">DECIMAL ??</a> | FunctionResult | ?????????????????????? |
| <a href="https://support.office.com/en-us/article/DEGREES-function-4d6ec4db-e694-4b94-ace0-1cc3f61f9ba1" target="_blank">DEGREES ??</a> | FunctionResult | ???????? |
| <a href="https://support.office.com/en-us/article/DELTA-function-2f763672-c959-4e07-ac33-fe03220ba432" target="_blank">DELTA ??</a> | FunctionResult | ????????? |
| <a href="https://support.office.com/en-us/article/DEVSQ-function-8b739616-8376-4df5-8bd0-cfe0a6caf444" target="_blank">DEVSQ ??</a> | FunctionResult | ??????? |
| <a href="https://support.office.com/en-us/article/DGET-function-455568bf-4eef-45f7-90f0-ec250d00892e" target="_blank">DGET ??</a> | FunctionResult | ?????????????????? |
| <a href="https://support.office.com/en-us/article/DISC-function-71fce9f3-3f05-4acf-a5a3-eac6ef4daa53" target="_blank">DISC ??</a> | FunctionResult | ???????? |
| <a href="https://support.office.com/en-us/article/DMAX-function-f4e8209d-8958-4c3d-a1ee-6351665d41c2" target="_blank">DMAX ??</a> | FunctionResult | ?????????????? |
| <a href="https://support.office.com/en-us/article/DMIN-function-4ae6f1d9-1f26-40f1-a783-6dc3680192a3" target="_blank">DMIN ??</a> | FunctionResult | ?????????????? |
| <a href="https://support.office.com/en-us/article/DOLLAR-function-a6cd05d9-9740-4ad3-a469-8109d18ff611" target="_blank">DOLLAR ??</a> | FunctionResult | ?? $???????????????? |
| <a href="https://support.office.com/en-us/article/DOLLARDE-function-db85aab0-1677-428a-9dfd-a38476693427" target="_blank">DOLLARDE ??</a> | FunctionResult | ?????????????????????? |
| <a href="https://support.office.com/en-us/article/DOLLARFR-function-0835d163-3023-4a33-9824-3042c5d4f495" target="_blank">DOLLARFR ??</a> | FunctionResult | ?????????????????????? |
| <a href="https://support.office.com/en-us/article/DPRODUCT-function-4f96b13e-d49c-47a7-b769-22f6d017cb31" target="_blank">DPRODUCT ??</a> | FunctionResult | ???????????????????????? |
| <a href="https://support.office.com/en-us/article/DSTDEV-function-026b8c73-616d-4b5e-b072-241871c4ab96" target="_blank">DSTDEV ??</a> | FunctionResult | ?????????????????????? |
| <a href="https://support.office.com/en-us/article/DSTDEVP-function-04b78995-da03-4813-bbd9-d74fd0f5d94b" target="_blank">DSTDEVP ??</a> | FunctionResult | ??????????????????????? |
| <a href="https://support.office.com/en-us/article/DSUM-function-53181285-0c4b-4f5a-aaa3-529a322be41b" target="_blank">DSUM ??</a> | FunctionResult | ???????????????????????? |
| <a href="https://support.office.com/en-us/article/DURATION-function-b254ea57-eadc-4602-a86a-c8e369334038" target="_blank">DURATION ??</a> | FunctionResult | ????????????????? |
| <a href="https://support.office.com/en-us/article/DVAR-function-d6747ca9-99c7-48bb-996e-9d7af00f3ed1" target="_blank">DVAR ??</a> | FunctionResult | ???????????????????? |
| <a href="https://support.office.com/en-us/article/DVARP-function-eb0ba387-9cb7-45c8-81e9-0394912502fc" target="_blank">DVARP ??</a> | FunctionResult | ??????????????????????? |
| <a href="https://support.office.com/en-us/article/EDATE-function-3c920eb2-6e66-44e7-a1f5-753ae47ee4f5" target="_blank">EDATE ??</a> | FunctionResult | ???????????????/????? |
| <a href="https://support.office.com/en-us/article/EFFECT-function-910d4e4c-79e2-4009-95e6-507e04f11bc4" target="_blank">EFFECT ??</a> | FunctionResult | ??????? |
| <a href="https://support.office.com/en-us/article/EOMONTH-function-7314ffa1-2bc9-4005-9d66-f49db127d628" target="_blank">EOMONTH ??</a> | FunctionResult | ?????????????????????????? |
| <a href="https://support.office.com/en-us/article/ERF-function-c53c7e7b-5482-4b6c-883e-56df3c9af349" target="_blank">ERF ??</a> | FunctionResult | ?????? |
| <a href="https://support.office.com/en-us/article/ERFPRECISE-function-9a349593-705c-4278-9a98-e4122831a8e0" target="_blank">ERF.PRECISE ??</a> | FunctionResult | ?????? |
| <a href="https://support.office.com/en-us/article/ERFC-function-736e0318-70ba-4e8b-8d08-461fe68b71b3" target="_blank">ERFC ??</a> | FunctionResult | ???????? |
| <a href="https://support.office.com/en-us/article/ERFCPRECISE-function-e90e6bab-f45e-45df-b2ac-cd2eb4d4a273" target="_blank">ERFC.PRECISE ??</a> | FunctionResult | ??? x ??????????? ERF ?? |
| <a href="https://support.office.com/en-us/article/ERRORTYPE-function-10958677-7c8d-44f7-ae77-b9a9ee6eefaa" target="_blank">ERROR.TYPE ??</a> | FunctionResult | ?????????????? |
| <a href="https://support.office.com/en-us/article/EVEN-function-197b5f06-c795-4c1e-8696-3c3b8a646cf9" target="_blank">EVEN ??</a> | FunctionResult | ????????????? |
| <a href="https://support.office.com/en-us/article/EXACT-function-d3087698-fc15-4a15-9631-12575cf29926" target="_blank">EXACT ??</a> | FunctionResult | ??????????? |
| <a href="https://support.office.com/en-us/article/EXP-function-c578f034-2c45-4c37-bc8c-329660a63abe" target="_blank">EXP ??</a> | FunctionResult | ?? e ? n ?? |
| <a href="https://support.office.com/en-us/article/EXPONDIST-function-4c12ae24-e563-4155-bf3e-8b78b6ae140e" target="_blank">EXPON.DIST ??</a> | FunctionResult | ?????? |
| <a href="https://support.office.com/en-us/article/FDIST-function-a887efdc-7c8e-46cb-a74a-f884cd29b25d" target="_blank">F.DIST ??</a> | FunctionResult | ?? F ???? |
| <a href="https://support.office.com/en-us/article/FDISTRT-function-d74cbb00-6017-4ac9-b7d7-6049badc0520" target="_blank">F.DIST.RT ??</a> | FunctionResult | ?? F ???? |
| <a href="https://support.office.com/en-us/article/FINV-function-0dda0cf9-4ea0-42fd-8c3c-417a1ff30dbe" target="_blank">F.INV ??</a> | FunctionResult | ?? F ????????? |
| <a href="https://support.office.com/en-us/article/FINVRT-function-d371aa8f-b0b1-40ef-9cc2-496f0693ac00" target="_blank">F.INV.RT ??</a> | FunctionResult | ?? F ????????? |
| <a href="https://support.office.com/en-us/article/FACT-function-ca8588c2-15f2-41c0-8e8c-c11bd471a4f3" target="_blank">FACT ??</a> | FunctionResult | ??????? |
| <a href="https://support.office.com/en-us/article/FACTDOUBLE-function-e67697ac-d214-48eb-b7b7-cce2589ecac8" target="_blank">FACTDOUBLE ??</a> | FunctionResult | ???????? |
| <a href="https://support.office.com/en-us/article/FALSE-function-2d58dfa5-9c03-4259-bf8f-f0ae14346904" target="_blank">FALSE ??</a> | FunctionResult | ????? `FALSE` |
| <a href="https://support.office.com/en-us/article/FIND-FINDB-functions-c7912941-af2a-4bdf-a553-d0d89b0a0628" target="_blank">FIND?FINDB ??</a> | FunctionResult | ??????????????????? |
| <a href="https://support.office.com/en-us/article/FISHER-function-d656523c-5076-4f95-b87b-7741bf236c69" target="_blank">FISHER ??</a> | FunctionResult | ?? Fisher ??? |
| <a href="https://support.office.com/en-us/article/FISHERINV-function-62504b39-415a-4284-a285-19c8e82f86bb" target="_blank">FISHERINV ??</a> | FunctionResult | ?? Fisher ???? |
| <a href="https://support.office.com/en-us/article/FIXED-function-ffd5723c-324c-45e9-8b96-e41be2a8274a" target="_blank">FIXED ??</a> | FunctionResult | ??????????????????? |
| <a href="https://support.office.com/en-us/article/FLOOR-function-14bb497c-24f2-4e04-b327-b0b4de5a8886" target="_blank">FLOOR ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/FLOORMATH-function-c302b599-fbdb-4177-ba19-2c2b1249a2f5" target="_blank">FLOOR.MATH ??</a> | FunctionResult | ???????????????????????? |
| <a href="https://support.office.com/en-us/article/FLOORPRECISE-function-f769b468-1452-4617-8dc3-02f842a0702e" target="_blank">FLOOR.PRECISE ??</a> | FunctionResult | ????????????????????????????????????????????? |
| <a href="https://support.office.com/en-us/article/FORECAST-function-50ca49c9-7b40-4892-94e4-7ad38bbeda99" target="_blank">FORECAST ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/FORECASTETS-function-897a2fe9-6595-4680-a0b0-93e0308d5f6e" target="_blank">FORECAST.ETS ??</a> | FunctionResult | ???????? (ETS) ??? AAA ???????????????????? |
| <a href="https://support.office.com/en-us/article/FORECASTETSCONFINT-function-897a2fe9-6595-4680-a0b0-93e0308d5f6e" target="_blank">FORECAST.ETS.CONFINT ??</a> | FunctionResult | ???????????????? |
| <a href="https://support.office.com/en-us/article/FORECASTETSSEASONALITY-function-897a2fe9-6595-4680-a0b0-93e0308d5f6e" target="_blank">FORECAST.ETS.SEASONALITY ??</a> | FunctionResult | ??????????? Excel ?????????? |
| <a href="https://support.office.com/en-us/article/FORECASTETSSTAT-function-897a2fe9-6595-4680-a0b0-93e0308d5f6e" target="_blank">FORECAST.ETS.STAT ??</a> | FunctionResult | ???????????????? |
| <a href="https://support.office.com/en-us/article/FORECASTLINEAR-function-897a2fe9-6595-4680-a0b0-93e0308d5f6e" target="_blank">FORECAST.LINEAR ??</a> | FunctionResult | ??????????? |
| <a href="https://support.office.com/en-us/article/FV-function-2eef9f44-a084-4c61-bdd8-4fe4bb1b71b3" target="_blank">FV ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/FVSCHEDULE-function-bec29522-bd87-4082-bab9-a241f3fb251d" target="_blank">FVSCHEDULE ??</a> | FunctionResult | ??????????????????? |
| <a href="https://support.office.com/en-us/article/GAMMA-function-ce1702b1-cf55-471d-8307-f83be0fc5297" target="_blank">GAMMA ??</a> | FunctionResult | ?? Gamma ??? |
| <a href="https://support.office.com/en-us/article/GAMMADIST-function-9b6f1538-d11c-4d5f-8966-21f6a2201def" target="_blank">GAMMA.DIST ??</a> | FunctionResult | ?? ? ?? |
| <a href="https://support.office.com/en-us/article/GAMMAINV-function-74991443-c2b0-4be5-aaab-1aa4d71fbb18" target="_blank">GAMMA.INV ??</a> | FunctionResult | ?? ? ???????? |
| <a href="https://support.office.com/en-us/article/GAMMALN-function-b838c48b-c65f-484f-9e1d-141c55470eb9" target="_blank">GAMMALN ??</a> | FunctionResult | ?? ? ??????? ?(x) |
| <a href="https://support.office.com/en-us/article/GAMMALNPRECISE-function-5cdfe601-4e1e-4189-9d74-241ef1caa599" target="_blank">GAMMALN.PRECISE ??</a> | FunctionResult | ?? ? ??????? ?(x) |
| <a href="https://support.office.com/en-us/article/GAUSS-function-069f1b4e-7dee-4d6a-a71f-4b69044a6b33" target="_blank">GAUSS ??</a> | FunctionResult | ???????????? 0.5 ?? |
| <a href="https://support.office.com/en-us/article/GCD-function-d5107a51-69e3-461f-8e4c-ddfc21b5073a" target="_blank">GCD ??</a> | FunctionResult | ??????? |
| <a href="https://support.office.com/en-us/article/GEOMEAN-function-db1ac48d-25a5-40a0-ab83-0b38980e40d5" target="_blank">GEOMEAN ??</a> | FunctionResult | ??????? |
| <a href="https://support.office.com/en-us/article/GESTEP-function-f37e7d2a-41da-4129-be95-640883fca9df" target="_blank">GESTEP ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/HARMEAN-function-5efd9184-fab5-42f9-b1d3-57883a1d3bc6" target="_blank">HARMEAN ??</a> | FunctionResult | ??????? |
| <a href="https://support.office.com/en-us/article/HEX2BIN-function-a13aafaa-5737-4920-8424-643e581828c1" target="_blank">HEX2BIN ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/HEX2DEC-function-8c8c3155-9f37-45a5-a3ee-ee5379ef106e" target="_blank">HEX2DEC ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/HEX2OCT-function-54d52808-5d19-4bd0-8a63-1096a5d11912" target="_blank">HEX2OCT ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/HLOOKUP-function-a3034eec-b719-4ba3-bb65-e1ad662ed95f" target="_blank">HLOOKUP ??</a> | FunctionResult | ??????????????????? |
| <a href="https://support.office.com/en-us/article/HOUR-function-a3afa879-86cb-4339-b1b5-2dd2d7310ac7" target="_blank">HOUR ??</a> | FunctionResult | ????????? |
| <a href="https://support.office.com/en-us/article/HYPERLINK-function-333c7ce6-c5ae-4164-9c47-7de9b76f577f" target="_blank">HYPERLINK ??</a> | FunctionResult | ??????????????????????????????? Internet ???? |
| <a href="https://support.office.com/en-us/article/HYPGEOMDIST-function-6dbd547f-1d12-4b1f-8ae5-b0d9e3d22fbf" target="_blank">HYPGEOM.DIST ??</a> | FunctionResult | ??????? |
| <a href="https://support.office.com/en-us/article/IF-function-69aed7c9-4e8a-4755-a9bc-aa8bbff73be2" target="_blank">IF ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/IMABS-function-b31e73c6-d90c-4062-90bc-8eb351d765a1" target="_blank">IMABS ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/IMAGINARY-function-dd5952fd-473d-44d9-95a1-9a17b23e428a" target="_blank">IMAGINARY ??</a> | FunctionResult | ????????? |
| <a href="https://support.office.com/en-us/article/IMARGUMENT-function-eed37ec1-23b3-4f59-b9f3-d340358a034a" target="_blank">IMARGUMENT ??</a> | FunctionResult | ????????? - ?? ? |
| <a href="https://support.office.com/en-us/article/IMCONJUGATE-function-2e2fc1ea-f32b-4f9b-9de6-233853bafd42" target="_blank">IMCONJUGATE ??</a> | FunctionResult | ????????? |
| <a href="https://support.office.com/en-us/article/IMCOS-function-dad75277-f592-4a6b-ad6c-be93a808a53c" target="_blank">IMCOS ??</a> | FunctionResult | ???????? |
| <a href="https://support.office.com/en-us/article/IMCOSH-function-053e4ddb-4122-458b-be9a-457c405e90ff" target="_blank">IMCOSH ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/IMCOT-function-dc6a3607-d26a-4d06-8b41-8931da36442c" target="_blank">IMCOT ??</a> | FunctionResult | ???????? |
| <a href="https://support.office.com/en-us/article/IMCSC-function-9e158d8f-2ddf-46cd-9b1d-98e29904a323" target="_blank">IMCSC ??</a> | FunctionResult | ???????? |
| <a href="https://support.office.com/en-us/article/IMCSCH-function-c0ae4f54-5f09-4fef-8da0-dc33ea2c5ca9" target="_blank">IMCSCH ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/IMDIV-function-a505aff7-af8a-4451-8142-77ec3d74d83f" target="_blank">IMDIV ??</a> | FunctionResult | ???????? |
| <a href="https://support.office.com/en-us/article/IMEXP-function-c6f8da1f-e024-4c0c-b802-a60e7147a95f" target="_blank">IMEXP ??</a> | FunctionResult | ???????? |
| <a href="https://support.office.com/en-us/article/IMLN-function-32b98bcf-8b81-437c-a636-6fb3aad509d8" target="_blank">IMLN ??</a> | FunctionResult | ????????? |
| <a href="https://support.office.com/en-us/article/IMLOG10-function-58200fca-e2a2-4271-8a98-ccd4360213a5" target="_blank">IMLOG10 ??</a> | FunctionResult | ??? 10 ???????? |
| <a href="https://support.office.com/en-us/article/IMLOG2-function-152e13b4-bc79-486c-a243-e6a676878c51" target="_blank">IMLOG2 ??</a> | FunctionResult | ??? 2 ???????? |
| <a href="https://support.office.com/en-us/article/IMPOWER-function-210fd2f5-f8ff-4c6a-9d60-30e34fbdef39" target="_blank">IMPOWER ??</a> | FunctionResult | ???????? |
| <a href="https://support.office.com/en-us/article/IMPRODUCT-function-2fb8651a-a4f2-444f-975e-8ba7aab3a5ba" target="_blank">IMPRODUCT ??</a> | FunctionResult | ??? 2 ? 255 ?????? |
| <a href="https://support.office.com/en-us/article/IMREAL-function-d12bc4c0-25d0-4bb3-a25f-ece1938bf366" target="_blank">IMREAL ??</a> | FunctionResult | ????????? |
| <a href="https://support.office.com/en-us/article/IMSEC-function-6df11132-4411-4df4-a3dc-1f17372459e0" target="_blank">IMSEC ??</a> | FunctionResult | ???????? |
| <a href="https://support.office.com/en-us/article/IMSECH-function-f250304f-788b-4505-954e-eb01fa50903b" target="_blank">IMSECH ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/IMSIN-function-1ab02a39-a721-48de-82ef-f52bf37859f6" target="_blank">IMSIN ??</a> | FunctionResult | ???????? |
| <a href="https://support.office.com/en-us/article/IMSINH-function-dfb9ec9e-8783-4985-8c42-b028e9e8da3d" target="_blank">IMSINH ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/IMSQRT-function-e1753f80-ba11-4664-a10e-e17368396b70" target="_blank">IMSQRT ??</a> | FunctionResult | ???????? |
| <a href="https://support.office.com/en-us/article/IMSUB-function-2e404b4d-4935-4e85-9f52-cb08b9a45054" target="_blank">IMSUB ??</a> | FunctionResult | ????????? |
| <a href="https://support.office.com/en-us/article/IMSUM-function-81542999-5f1c-4da6-9ffe-f1d7aaa9457f" target="_blank">IMSUM ??</a> | FunctionResult | ?????? |
| <a href="https://support.office.com/en-us/article/IMTAN-function-8478f45d-610a-43cf-8544-9fc0b553a132" target="_blank">IMTAN ??</a> | FunctionResult | ???????? |
| <a href="https://support.office.com/en-us/article/INT-function-a6c4af9e-356d-4369-ab6a-cb1fd9d343ef" target="_blank">INT ??</a> | FunctionResult | ?????????????? |
| <a href="https://support.office.com/en-us/article/INTRATE-function-5cb34dde-a221-4cb6-b3eb-0b9e55e1316f" target="_blank">INTRATE ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/IPMT-function-5cce0ad6-8402-4a41-8d29-61a0b054cb6f" target="_blank">IPMT ??</a> | FunctionResult | ??????????????? |
| <a href="https://support.office.com/en-us/article/IRR-function-64925eaa-9988-495b-b290-3ad0c163c1bc" target="_blank">IRR ??</a> | FunctionResult | ?????????????? |
| <a href="https://support.office.com/en-us/article/ISERR-function-0f2d7971-6019-40a0-a171-f2d869135665" target="_blank">ISERR ??</a> | FunctionResult | ????? #N/A ?????????`TRUE` |
| <a href="https://support.office.com/en-us/article/ISERROR-function-0f2d7971-6019-40a0-a171-f2d869135665" target="_blank">ISERROR ??</a> | FunctionResult | ????????????`TRUE` |
| <a href="https://support.office.com/en-us/article/ISEVEN-function-aa15929a-d77b-4fbb-92f4-2f479af55356" target="_blank">ISEVEN ??</a> | FunctionResult | ?????????`TRUE` |
| <a href="https://support.office.com/en-us/article/ISFORMULA-function-e4d1355f-7121-4ef2-801e-3839bfd6b1e5" target="_blank">ISFORMULA ??</a> | FunctionResult | ???????????????????`TRUE` |
| <a href="https://support.office.com/en-us/article/ISLOGICAL-function-0f2d7971-6019-40a0-a171-f2d869135665" target="_blank">ISLOGICAL ??</a> | FunctionResult | ??????????`TRUE` |
| <a href="https://support.office.com/en-us/article/ISNA-function-0f2d7971-6019-40a0-a171-f2d869135665" target="_blank">ISNA ??</a> | FunctionResult | ???? #N/A ??????`TRUE` |
| <a href="https://support.office.com/en-us/article/ISNONTEXT-function-0f2d7971-6019-40a0-a171-f2d869135665" target="_blank">ISNONTEXT ??</a> | FunctionResult | ??????????`TRUE` |
| <a href="https://support.office.com/en-us/article/ISNUMBER-function-0f2d7971-6019-40a0-a171-f2d869135665" target="_blank">ISNUMBER ??</a> | FunctionResult | ?????????`TRUE` |
| <a href="https://support.office.com/en-us/article/ISOCEILING-function-e587bb73-6cc2-4113-b664-ff5b09859a83" target="_blank">ISO.CEILING ??</a> | FunctionResult | ???????????????????????? |
| <a href="https://support.office.com/en-us/article/ISODD-function-0f2d7971-6019-40a0-a171-f2d869135665" target="_blank">ISODD ??</a> | FunctionResult | ?????????`TRUE` |
| <a href="https://support.office.com/en-us/article/ISOWEEKNUM-function-1c2d0afe-d25b-4ab1-8894-8d0520e90e0e" target="_blank">ISOWEEKNUM ??</a> | FunctionResult | ?????????? ISO ????? |
| <a href="https://support.office.com/en-us/article/ISPMT-function-fa58adb6-9d39-4ce0-8f43-75399cea56cc" target="_blank">ISPMT ??</a> | FunctionResult | ?????????????? |
| <a href="https://support.office.com/en-us/article/ISREF-function-0f2d7971-6019-40a0-a171-f2d869135665" target="_blank">ISREF ??</a> | FunctionResult | ?????????`TRUE` |
| <a href="https://support.office.com/en-us/article/ISTEXT-function-0f2d7971-6019-40a0-a171-f2d869135665" target="_blank">ISTEXT ??</a> | FunctionResult | ?????????`TRUE` |
| <a href="https://support.office.com/en-us/article/KURT-function-bc3a265c-5da4-4dcb-b7fd-c237789095ab" target="_blank">KURT ??</a> | FunctionResult | ????????? |
| <a href="https://support.office.com/en-us/article/LARGE-function-3af0af19-1190-42bb-bb8b-01672ec00a64" target="_blank">LARGE ??</a> | FunctionResult | ??????? k ???? |
| <a href="https://support.office.com/en-us/article/LCM-function-7152b67a-8bb5-4075-ae5c-06ede5563c94" target="_blank">LCM ??</a> | FunctionResult | ??????? |
| <a href="https://support.office.com/en-us/article/LEFT-LEFTB-functions-9203d2d2-7960-479b-84c6-1ea52b99640c" target="_blank">LEFT?LEFTB ??</a> | FunctionResult | ????????????? |
| <a href="https://support.office.com/en-us/article/LEN-LENB-functions-29236f94-cedc-429d-affd-b5e33d2c67cb" target="_blank">LEN?LENB ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/LN-function-81fe1ed7-dac9-4acd-ba1d-07a142c6118f" target="_blank">LN ??</a> | FunctionResult | ????????? |
| <a href="https://support.office.com/en-us/article/LOG-function-4e82f196-1ca9-4747-8fb0-6c4a3abb3280" target="_blank">LOG ??</a> | FunctionResult | ????????????? |
| <a href="https://support.office.com/en-us/article/LOG10-function-c75b881b-49dd-44fb-b6f4-37e3486a0211" target="_blank">LOG10 ??</a> | FunctionResult | ??? 10 ????? |
| <a href="https://support.office.com/en-us/article/LOGNORMDIST-function-eb60d00b-48a9-4217-be2b-6074aee6b070" target="_blank">LOGNORM.DIST ??</a> | FunctionResult | ???????? |
| <a href="https://support.office.com/en-us/article/LOGNORMINV-function-fe79751a-f1f2-4af8-a0a1-e151b2d4f600" target="_blank">LOGNORM.INV ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/LOOKUP-function-446d94af-663b-451d-8251-369d5e3864cb" target="_blank">LOOKUP ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/LOWER-function-3f21df02-a80c-44b2-afaf-81358f9fdeb4" target="_blank">LOWER ??</a> | FunctionResult | ???????? |
| <a href="https://support.office.com/en-us/article/MATCH-function-e8dffd45-c762-47d6-bf89-533f4a37673a" target="_blank">MATCH ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/MAX-function-e0012414-9ac8-4b34-9a47-73e662c08098" target="_blank">MAX ??</a> | FunctionResult | ??????????? |
| <a href="https://support.office.com/en-us/article/MAXA-function-814bda1e-3840-4bff-9365-2f59ac2ee62d" target="_blank">MAXA ??</a> | FunctionResult | ??????????????????????? |
| <a href="https://support.office.com/en-us/article/MDURATION-function-b3786a69-4f20-469a-94ad-33e5b90a763c" target="_blank">MDURATION ??</a> | FunctionResult | ??????? 100 ??????????????? |
| <a href="https://support.office.com/en-us/article/MEDIAN-function-d0916313-4753-414c-8537-ce85bdd967d2" target="_blank">MEDIAN ??</a> | FunctionResult | ????????? |
| <a href="https://support.office.com/en-us/article/MID-MIDB-functions-d5f9e25c-d7d6-472e-b568-4ecb12433028" target="_blank">MID?MIDB ??</a> | FunctionResult | ???????????????????????? |
| <a href="https://support.office.com/en-us/article/MIN-function-61635d12-920f-4ce2-a70f-96f202dcc152" target="_blank">MIN ??</a> | FunctionResult | ??????????? |
| <a href="https://support.office.com/en-us/article/MINA-function-245a6f46-7ca5-4dc7-ab49-805341bc31d3" target="_blank">MINA ??</a> | FunctionResult | ??????????????????????? |
| <a href="https://support.office.com/en-us/article/MINUTE-function-af728df0-05c4-4b07-9eed-a84801a60589" target="_blank">MINUTE ??</a> | FunctionResult | ????????? |
| <a href="https://support.office.com/en-us/article/MIRR-function-b020f038-7492-4fb4-93c1-35c345b53524" target="_blank">MIRR ??</a> | FunctionResult | ??????????????????????????? |
| <a href="https://support.office.com/en-us/article/MOD-function-9b6cd169-b6ee-406a-a97b-edf2a9dc24f3" target="_blank">MOD ??</a> | FunctionResult | ??????? |
| <a href="https://support.office.com/en-us/article/MONTH-function-579a2881-199b-48b2-ab90-ddba0eba86e8" target="_blank">MONTH ??</a> | FunctionResult | ???????? |
| <a href="https://support.office.com/en-us/article/MROUND-function-c299c3b0-15a5-426d-aa4b-d2d5b3baf427" target="_blank">MROUND ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/MULTINOMIAL-function-6fa6373c-6533-41a2-a45e-a56db1db1bf6" target="_blank">MULTINOMIAL ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/N-function-a624cad1-3635-4208-b54a-29733d1278c9" target="_blank">N ??</a> | FunctionResult | ????????? |
| <a href="https://support.office.com/en-us/article/NA-function-5469c2d1-a90c-4fb5-9bbc-64bd9bb6b47c" target="_blank">NA ??</a> | FunctionResult | ????? #N/A |
| <a href="https://support.office.com/en-us/article/NEGBINOMDIST-function-c8239f89-c2d0-45bd-b6af-172e570f8599" target="_blank">NEGBINOM.DIST ??</a> | FunctionResult | ??????????? |
| <a href="https://support.office.com/en-us/article/NETWORKDAYS-function-48e717bf-a7a3-495f-969e-5005e3eb18e7" target="_blank">NETWORKDAYS ??</a> | FunctionResult | ??????????????? |
| <a href="https://support.office.com/en-us/article/NETWORKDAYSINTL-function-a9b26239-4f20-46a1-9ab8-4e925bfd5e28" target="_blank">NETWORKDAYS.INTL ??</a> | FunctionResult | ??????????????????????????????????? |
| <a href="https://support.office.com/en-us/article/NOMINAL-function-7f1ae29b-6b92-435e-b950-ad8b190ddd2b" target="_blank">NOMINAL ??</a> | FunctionResult | ??????? |
| <a href="https://support.office.com/en-us/article/NORMDIST-function-edb1cc14-a21c-4e53-839d-8082074c9f8d" target="_blank">NORM.DIST ??</a> | FunctionResult | ????????? |
| <a href="https://support.office.com/en-us/article/NORMINV-function-54b30935-fee7-493c-bedb-2278a9db7e13" target="_blank">NORM.INV ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/NORMSDIST-function-1e787282-3832-4520-a9ae-bd2a8d99ba88" target="_blank">NORM.S.DIST ??</a> | FunctionResult | ??????????? |
| <a href="https://support.office.com/en-us/article/NORMSINV-function-d6d556b4-ab7f-49cd-b526-5a20918452b1" target="_blank">NORM.S.INV ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/NOT-function-9cfc6011-a054-40c7-a140-cd4ba2d87d77" target="_blank">NOT ??</a> | FunctionResult | ???????? |
| <a href="https://support.office.com/en-us/article/NOW-function-3337fd29-145a-4347-b2e6-20c904739c46" target="_blank">NOW ??</a> | FunctionResult | ????????????? |
| <a href="https://support.office.com/en-us/article/NPER-function-240535b5-6653-4d2d-bfcf-b6a38151d815" target="_blank">NPER ??</a> | FunctionResult | ??????????? |
| <a href="https://support.office.com/en-us/article/NPV-function-8672cb67-2576-4d07-b67b-ac28acf2a568" target="_blank">NPV ??</a> | FunctionResult | ?????????????????????? |
| <a href="https://support.office.com/en-us/article/NUMBERVALUE-function-1b05c8cf-2bfa-4437-af70-596c7ea7d879" target="_blank">NUMBERVALUE ??</a> | FunctionResult | ??????????????????? |
| <a href="https://support.office.com/en-us/article/OCT2BIN-function-55383471-3c56-4d27-9522-1a8ec646c589" target="_blank">OCT2BIN ??</a> | FunctionResult | ??????????? |
| <a href="https://support.office.com/en-us/article/OCT2DEC-function-87606014-cb98-44b2-8dbb-e48f8ced1554" target="_blank">OCT2DEC ??</a> | FunctionResult | ??????????? |
| <a href="https://support.office.com/en-us/article/OCT2HEX-function-912175b4-d497-41b4-a029-221f051b858f" target="_blank">OCT2HEX ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/ODD-function-deae64eb-e08a-4c88-8b40-6d0b42575c98" target="_blank">ODD ??</a> | FunctionResult | ?????????????? |
| <a href="https://support.office.com/en-us/article/ODDFPRICE-function-d7d664a8-34df-4233-8d2b-922bcf6a69e1" target="_blank">ODDFPRICE ??</a> | FunctionResult | ??????? 100 ?????????????? |
| <a href="https://support.office.com/en-us/article/ODDFYIELD-function-66bc8b7b-6501-4c93-9ce3-2fd16220fe37" target="_blank">ODDFYIELD ??</a> | FunctionResult | ?????????????? |
| <a href="https://support.office.com/en-us/article/ODDLPRICE-function-fb657749-d200-4902-afaf-ed5445027fc4" target="_blank">ODDLPRICE ??</a> | FunctionResult | ??????? 100 ??????????????? |
| <a href="https://support.office.com/en-us/article/ODDLYIELD-function-c873d088-cf40-435f-8d41-c8232fee9238" target="_blank">ODDLYIELD ??</a> | FunctionResult | ??????????????? |
| <a href="https://support.office.com/en-us/article/OR-function-7d17ad14-8700-4281-b308-00b131e22af0" target="_blank">OR ??</a> | FunctionResult | ??????? true???`TRUE` |
| <a href="https://support.office.com/en-us/article/PDURATION-function-44f33460-5be5-4c90-b857-22308892adaf" target="_blank">PDURATION ??</a> | FunctionResult | ??????????????? |
| <a href="https://support.office.com/en-us/article/PERCENTILEEXC-function-bbaa7204-e9e1-4010-85bf-c31dc5dce4ba" target="_blank">PERCENTILE.EXC ??</a> | FunctionResult | ????? K ?????K ?? 0 ? 1 ????? 0 ? 1 |
| <a href="https://support.office.com/en-us/article/PERCENTILEINC-function-680f9539-45eb-410b-9a5e-c1355e5fe2ed" target="_blank">PERCENTILE.INC ??</a> | FunctionResult | ????? K ???? |
| <a href="https://support.office.com/en-us/article/PERCENTRANKEXC-function-d8afee96-b7e2-4a2f-8c01-8fcdedaa6314" target="_blank">PERCENTRANK.EXC ??</a> | FunctionResult | ?????????????????????? 0 ? 1 ????? 0 ? 1? |
| <a href="https://support.office.com/en-us/article/PERCENTRANKINC-function-149592c9-00c0-49ba-86c1-c1f45b80463a" target="_blank">PERCENTRANK.INC ??</a> | FunctionResult | ??????????????? |
| <a href="https://support.office.com/en-us/article/PERMUT-function-3bd1cb9a-2880-41ab-a197-f246a7a602d3" target="_blank">PERMUT ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/PERMUTATIONA-function-6c7d7fdc-d657-44e6-aa19-2857b25cae4e" target="_blank">PERMUTATIONA ??</a> | FunctionResult | ?????????????????????????????? |
| <a href="https://support.office.com/en-us/article/PHI-function-23e49bc6-a8e8-402d-98d3-9ded87f6295c" target="_blank">PHI ??</a> | FunctionResult | ?????????????? |
| <a href="https://support.office.com/en-us/article/PI-function-264199d0-a3ba-46b8-975a-c4a04608989b" target="_blank">PI ??</a> | FunctionResult | ?? pi ? |
| <a href="https://support.office.com/en-us/article/PMT-function-0214da64-9a63-4996-bc20-214433fa6441" target="_blank">PMT ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/POISSONDIST-function-8fe148ff-39a2-46cb-abf3-7772695d9636" target="_blank">POISSON.DIST ??</a> | FunctionResult | ?????? |
| <a href="https://support.office.com/en-us/article/POWER-function-d3f2908b-56f4-4c3f-895a-07fb519c362a" target="_blank">POWER ??</a> | FunctionResult | ????????? |
| <a href="https://support.office.com/en-us/article/PPMT-function-c370d9e3-7749-4ca4-beea-b06c6ac95e1b" target="_blank">PPMT ??</a> | FunctionResult | ????????????????? |
| <a href="https://support.office.com/en-us/article/PRICE-function-3ea9deac-8dfa-436f-a7c8-17ea02c21b0a" target="_blank">PRICE ??</a> | FunctionResult | ??????? 100 ?????????????? |
| <a href="https://support.office.com/en-us/article/PRICEDISC-function-d06ad7c1-380e-4be7-9fd9-75e3079acfd3" target="_blank">PRICEDISC ??</a> | FunctionResult | ??????? 100 ?????????? |
| <a href="https://support.office.com/en-us/article/PRICEMAT-function-52c3b4da-bc7e-476a-989f-a95f675cae77" target="_blank">PRICEMAT ??</a> | FunctionResult | ??????? 100 ???????????????? |
| <a href="https://support.office.com/en-us/article/PROB-function-9ac30561-c81c-4259-8253-34f0a238fc49" target="_blank">PROB ??</a> | FunctionResult | ?????????????????????? |
| <a href="https://support.office.com/en-us/article/PRODUCT-function-8e6b5b24-90ee-4650-aeec-80982a0512ce" target="_blank">PRODUCT ??</a> | FunctionResult | ?????? |
| <a href="https://support.office.com/en-us/article/PROPER-function-52a5a283-e8b2-49be-8506-b2887b889f94" target="_blank">PROPER ??</a> | FunctionResult | ???????????????? |
| <a href="https://support.office.com/en-us/article/PV-function-23879d31-0e02-4321-be01-da16e8168cbd" target="_blank">PV ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/QUARTILEEXC-function-5a355b7a-840b-4a01-b0f1-f538c2864cad" target="_blank">QUARTILE.EXC ??</a> | FunctionResult | ??? 0 ? 1 ????? 0 ? 1?????????????????? |
| <a href="https://support.office.com/en-us/article/QUARTILEINC-function-1bbacc80-5075-42f1-aed6-47d735c4819d" target="_blank">QUARTILE.INC ??</a> | FunctionResult | ??????????? |
| <a href="https://support.office.com/en-us/article/QUOTIENT-function-9f7bf099-2a18-4282-8fa4-65290cc99dee" target="_blank">QUOTIENT ??</a> | FunctionResult | ??????????? |
| <a href="https://support.office.com/en-us/article/RADIANS-function-ac409508-3d48-45f5-ac02-1497c92de5bf" target="_blank">RADIANS ??</a> | FunctionResult | ??????? |
| <a href="https://support.office.com/en-us/article/RAND-function-4cbfa695-8869-4788-8d90-021ea9f5be73" target="_blank">RAND ??</a> | FunctionResult | ?? 0 ? 1 ???????? |
| <a href="https://support.office.com/en-us/article/RANDBETWEEN-function-4cc7f0d1-87dc-4eb7-987f-a469ab381685" target="_blank">RANDBETWEEN ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/RANKAVG-function-bd406a6f-eb38-4d73-aa8e-6d1c3c72e83a" target="_blank">RANK.AVG ??</a> | FunctionResult | ?????????????? |
| <a href="https://support.office.com/en-us/article/RANKEQ-function-284858ce-8ef6-450e-b662-26245be04a40" target="_blank">RANK.EQ ??</a> | FunctionResult | ?????????????? |
| <a href="https://support.office.com/en-us/article/RATE-function-9f665657-4a7e-4bb7-a030-83fc59e748ce" target="_blank">RATE ??</a> | FunctionResult | ????????? |
| <a href="https://support.office.com/en-us/article/RECEIVED-function-7a3f8b93-6611-4f81-8576-828312c9b5e5" target="_blank">RECEIVED ??</a> | FunctionResult | ???????????????? |
| <a href="https://support.office.com/en-us/article/REPLACE-REPLACEB-functions-8d799074-2425-4a8a-84bc-82472868878a" target="_blank">REPLACE?REPLACEB ??</a> | FunctionResult | ???????? |
| <a href="https://support.office.com/en-us/article/REPT-function-04c4d778-e712-43b4-9c15-d656582bb061" target="_blank">REPT ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/RIGHT-RIGHTB-functions-240267ee-9afa-4639-a02b-f19e1786cf2f" target="_blank">RIGHT?RIGHTB ??</a> | FunctionResult | ????????????? |
| <a href="https://support.office.com/en-us/article/ROMAN-function-d6b0b99e-de46-4704-a518-b45a0f8b56f5" target="_blank">ROMAN ??</a> | FunctionResult | ?????????????????? |
| <a href="https://support.office.com/en-us/article/ROUND-function-c018c5d8-40fb-4053-90b1-b3e7f61a213c" target="_blank">ROUND ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/ROUNDDOWN-function-2ec94c73-241f-4b01-8c6f-17e6d7968f53" target="_blank">ROUNDDOWN ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/ROUNDUP-function-f8bc9b23-e795-47db-8703-db171d0c42a7" target="_blank">ROUNDUP ??</a> | FunctionResult | ?????????????? |
| <a href="https://support.office.com/en-us/article/ROWS-function-b592593e-3fc2-47f2-bec1-bda493811597" target="_blank">ROWS ??</a> | FunctionResult | ???????? |
| <a href="https://support.office.com/en-us/article/RRI-function-6f5822d8-7ef1-4233-944c-79e8172930f4" target="_blank">RRI ??</a> | FunctionResult | ????????????? |
| <a href="https://support.office.com/en-us/article/RTD-function-e0cc001a-56f0-470a-9b19-9455dc0eb593" target="_blank">RTD ??</a> | FunctionResult | ????? COM ????????????? |
| <a href="https://support.office.com/en-us/article/SEC-function-ff224717-9c87-4170-9b58-d069ced6d5f7" target="_blank">SEC ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/SECH-function-e05a789f-5ff7-4d7f-984a-5edb9b09556f" target="_blank">SECH ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/SECOND-function-740d1cfc-553c-4099-b668-80eaa24e8af1" target="_blank">SECOND ??</a> | FunctionResult | ???????? |
| <a href="https://support.office.com/en-us/article/SERIESSUM-function-a3ab25b5-1093-4f5b-b084-96c49087f637" target="_blank">SERIESSUM ??</a> | FunctionResult | ?????????????? |
| <a href="https://support.office.com/en-us/article/SHEET-function-44718b6f-8b87-47a1-a9d6-b701c06cff24" target="_blank">SHEET ??</a> | FunctionResult | ?????????????? |
| <a href="https://support.office.com/en-us/article/SHEETS-function-770515eb-e1e8-45ce-8066-b557e5e4b80b" target="_blank">SHEETS ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/SIGN-function-109c932d-fcdc-4023-91f1-2dd0e916a1d8" target="_blank">SIGN ??</a> | FunctionResult | ??????? |
| <a href="https://support.office.com/en-us/article/SIN-function-cf0e3432-8b9e-483c-bc55-a76651c95602" target="_blank">SIN ??</a> | FunctionResult | ????????? |
| <a href="https://support.office.com/en-us/article/SINH-function-1e4e8b9f-2b65-43fc-ab8a-0a37f4081fa7" target="_blank">SINH ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/SKEW-function-bdf49d86-b1ef-4804-a046-28eaea69c9fa" target="_blank">SKEW ??</a> | FunctionResult | ??????????? |
| <a href="https://support.office.com/en-us/article/SKEWP-function-76530a5c-99b9-48a1-8392-26632d542fcb" target="_blank">SKEW.P ??</a> | FunctionResult | ???????????????????????????????????? |
| <a href="https://support.office.com/en-us/article/SLN-function-cdb666e5-c1c6-40a7-806a-e695edc2f1c8" target="_blank">SLN ??</a> | FunctionResult | ???????????????? |
| <a href="https://support.office.com/en-us/article/SMALL-function-17da8222-7c82-42b2-961b-14c45384df07" target="_blank">SMALL ??</a> | FunctionResult | ??????? k ???? |
| <a href="https://support.office.com/en-us/article/SQRT-function-654975c2-05c4-4831-9a24-2c65e4040fdf" target="_blank">SQRT ??</a> | FunctionResult | ?????? |
| <a href="https://support.office.com/en-us/article/SQRTPI-function-1fb4e63f-9b51-46d6-ad68-b3e7a8b519b4" target="_blank">SQRTPI ??</a> | FunctionResult | ????? * pi????? |
| <a href="https://support.office.com/en-us/article/STANDARDIZE-function-81d66554-2d54-40ec-ba83-6437108ee775" target="_blank">STANDARDIZE ??</a> | FunctionResult | ????????? |
| <a href="https://support.office.com/en-us/article/STDEVP-function-6e917c05-31a0-496f-ade7-4f4e7462f285" target="_blank">STDEV.P ??</a> | FunctionResult | ?????????????? |
| <a href="https://support.office.com/en-us/article/STDEVS-function-7d69cf97-0c1f-4acf-be27-f3e83904cc23" target="_blank">STDEV.S ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/STDEVA-function-5ff38888-7ea5-48de-9a6d-11ed73b29e9d" target="_blank">STDEVA ??</a> | FunctionResult | ?????????????????????? |
| <a href="https://support.office.com/en-us/article/STDEVPA-function-5578d4d6-455a-4308-9991-d405afe2c28c" target="_blank">STDEVPA ??</a> | FunctionResult | ?????????????????????????? |
| <a href="https://support.office.com/en-us/article/SUBSTITUTE-function-6434944e-a904-4336-a9b0-1e58df3bc332" target="_blank">SUBSTITUTE ??</a> | FunctionResult | ??????????????? |
| <a href="https://support.office.com/en-us/article/SUBTOTAL-function-7b027003-f060-4ade-9040-e478765b9939" target="_blank">SUBTOTAL ??</a> | FunctionResult | ????????????????? |
| <a href="https://support.office.com/en-us/article/SUM-function-043e1c7d-7726-4e80-8f32-07b23e057f89" target="_blank">SUM ??</a> | FunctionResult | ????? |
| <a href="https://support.office.com/en-us/article/SUMIF-function-169b8c99-c05c-4483-a712-1697a653039b" target="_blank">SUMIF ??</a> | FunctionResult | ????????????????? |
| <a href="https://support.office.com/en-us/article/SUMIFS-function-c9e748f5-7ea7-455d-9406-611cebce642b" target="_blank">SUMIFS ??</a> | FunctionResult | ???????????????? |
| <a href="https://support.office.com/en-us/article/SUMSQ-function-e3313c02-51cc-4963-aae6-31442d9ec307" target="_blank">SUMSQ ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/SYD-function-069f8106-b60b-4ca2-98e0-2a0f206bdb27" target="_blank">SYD ??</a> | FunctionResult | ??????????????????? |
| <a href="https://support.office.com/en-us/article/T-function-fb83aeec-45e7-4924-af95-53e073541228" target="_blank">T ??</a> | FunctionResult | ????????? |
| <a href="https://support.office.com/en-us/article/TDIST-function-4329459f-ae91-48c2-bba8-1ead1c6c21b2" target="_blank">T.DIST ??</a> | FunctionResult | ???? t ?????????? |
| <a href="https://support.office.com/en-us/article/TDIST2T-function-198e9340-e360-4230-bd21-f52f22ff5c28" target="_blank">T.DIST.2T ??</a> | FunctionResult | ???? t ?????????? |
| <a href="https://support.office.com/en-us/article/TDISTRT-function-20a30020-86f9-4b35-af1f-7ef6ae683eda" target="_blank">T.DIST.RT ??</a> | FunctionResult | ????? t ?? |
| <a href="https://support.office.com/en-us/article/TINV-function-2908272b-4e61-4942-9df9-a25fec9b0e2e" target="_blank">T.INV ??</a> | FunctionResult | ??????????????? t ??? t ? |
| <a href="https://support.office.com/en-us/article/TINV2T-function-ce72ea19-ec6c-4be7-bed2-b9baf2264f17" target="_blank">T.INV.2T ??</a> | FunctionResult | ???? t ?????? |
| <a href="https://support.office.com/en-us/article/TAN-function-08851a40-179f-4052-b789-d7f699447401" target="_blank">TAN ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/TANH-function-017222f0-a0c3-4f69-9787-b3202295dc6c" target="_blank">TANH ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/TBILLEQ-function-2ab72d90-9b4d-4efe-9fc2-0f81f2c19c8c" target="_blank">TBILLEQ ??</a> | FunctionResult | ?????????????? |
| <a href="https://support.office.com/en-us/article/TBILLPRICE-function-eacca992-c29d-425a-9eb8-0513fe6035a2" target="_blank">TBILLPRICE ??</a> | FunctionResult | ??????? 100 ?????????? |
| <a href="https://support.office.com/en-us/article/TBILLYIELD-function-6d381232-f4b0-4cd5-8e97-45b9c03468ba" target="_blank">TBILLYIELD ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/TEXT-function-20d5ac4d-7b94-49fd-bb38-93d29371225c" target="_blank">TEXT ??</a> | FunctionResult | ?????????????? |
| <a href="https://support.office.com/en-us/article/TIME-function-9a5aff99-8f7d-4611-845e-747d0b8d5457" target="_blank">TIME ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/TIMEVALUE-function-0b615c12-33d8-4431-bf3d-f3eb6d186645" target="_blank">TIMEVALUE ??</a> | FunctionResult | ??????????????? |
| <a href="https://support.office.com/en-us/article/TODAY-function-5eb3078d-a82c-4736-8930-2f51a028fdd9" target="_blank">TODAY ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/TRIM-function-410388fa-c5df-49c6-b16c-9e5630b479f9" target="_blank">TRIM ??</a> | FunctionResult | ???????? |
| <a href="https://support.office.com/en-us/article/TRIMMEAN-function-d90c9878-a119-4746-88fa-63d988f511d3" target="_blank">TRIMMEAN ??</a> | FunctionResult | ??????????? |
| <a href="https://support.office.com/en-us/article/TRUE-function-7652c6e3-8987-48d0-97cd-ef223246b3fb" target="_blank">TRUE ??</a> | FunctionResult | ????? `TRUE` |
| <a href="https://support.office.com/en-us/article/TRUNC-function-8b86a64c-3127-43db-ba14-aa5ceb292721" target="_blank">TRUNC ??</a> | FunctionResult | ???????? |
| <a href="https://support.office.com/en-us/article/TYPE-function-45b4e688-4bc3-48b3-a105-ffa892995899" target="_blank">TYPE ??</a> | FunctionResult | ??????????????? |
| <a href="https://support.office.com/en-us/article/UNICHAR-function-ffeb64f5-f131-44c6-b332-5cd72f0659b8" target="_blank">UNICHAR ??</a> | FunctionResult | ????????? Unicode ?? |
| <a href="https://support.office.com/en-us/article/UNICODE-function-adb74aaa-a2a5-4dde-aff6-966e4e81f16f" target="_blank">UNICODE ??</a> | FunctionResult | ????????????????????? |
| <a href="https://support.office.com/en-us/article/UPPER-function-c11f29b3-d1a3-4537-8df6-04d0049963d6" target="_blank">UPPER ??</a> | FunctionResult | ???????? |
| <a href="https://support.office.com/en-us/article/VALUE-function-257d0108-07dc-437d-ae1c-bc2d3953d8c2" target="_blank">VALUE ??</a> | FunctionResult | ?????????? |
| <a href="https://support.office.com/en-us/article/VARP-function-73d1285c-108c-4843-ba5d-a51f90656f3a" target="_blank">VAR.P ??</a> | FunctionResult | ???????????? |
| <a href="https://support.office.com/en-us/article/VARS-function-913633de-136b-449d-813e-65a00b2b990b" target="_blank">VAR.S ??</a> | FunctionResult | ???????? |
| <a href="https://support.office.com/en-us/article/VARA-function-3de77469-fa3a-47b4-85fd-81758a1e1d07" target="_blank">VARA ??</a> | FunctionResult | ???????????????????? |
| <a href="https://support.office.com/en-us/article/VARPA-function-59a62635-4e89-4fad-88ac-ce4dc0513b96" target="_blank">VARPA ??</a> | FunctionResult | ???????????????????????? |
| <a href="https://support.office.com/en-us/article/VDB-function-dde4e207-f3fa-488d-91d2-66d55e861d73" target="_blank">VDB ??</a> | FunctionResult | ??????????????????????????? |
| <a href="https://support.office.com/en-us/article/VLOOKUP-function-0bbc8083-26fe-4963-8ab8-93a18ad188a1" target="_blank">VLOOKUP ??</a> | FunctionResult | ????????????????????? |
| <a href="https://support.office.com/en-us/article/WEEKDAY-function-60e44483-2ed1-439f-8bd0-e404c190949a" target="_blank">WEEKDAY ??</a> | FunctionResult | ?????????????? |
| <a href="https://support.office.com/en-us/article/WEEKNUM-function-e5c43a03-b4ab-426c-b411-b18c13c75340" target="_blank">WEEKNUM ??</a> | FunctionResult | ?????????????????? |
| <a href="https://support.office.com/en-us/article/WEIBULLDIST-function-4e783c39-9325-49be-bbc9-a83ef82b45db" target="_blank">WEIBULL.DIST ??</a> | FunctionResult | ?? Weibull ?? |
| <a href="https://support.office.com/en-us/article/WORKDAY-function-f764a5b7-05fc-4494-9486-60d494efbf33" target="_blank">WORKDAY ??</a> | FunctionResult | ??????????????/??????????? |
| <a href="https://support.office.com/en-us/article/WORKDAYINTL-function-a378391c-9ba7-4678-8a39-39611a9bf81d" target="_blank">WORKDAY.INTL ??</a> | FunctionResult | ??????????????/??????????????????????????????? |
| <a href="https://support.office.com/en-us/article/XIRR-function-de1242ec-6477-445b-b11b-a303ad9adc9d" target="_blank">XIRR ??</a> | FunctionResult | ?????????????????????????? |
| <a href="https://support.office.com/en-us/article/XNPV-function-1b42bbf6-370f-4532-a0eb-d67c16b664b7" target="_blank">XNPV ??</a> | FunctionResult | ???????????????????????? |
| <a href="https://support.office.com/en-us/article/XOR-function-1548d4c2-5e47-4f77-9a92-0533bba14f37" target="_blank">XOR ??</a> | FunctionResult | ?????????????? |
| <a href="https://support.office.com/en-us/article/YEAR-function-c64f017a-1354-490d-981f-578e8ec8d3b9" target="_blank">YEAR ??</a> | FunctionResult | ???????? |
| <a href="https://support.office.com/en-us/article/YEARFRAC-function-3844141e-c76d-4143-82b6-208454ddc6a8" target="_blank">YEARFRAC ??</a> | FunctionResult | ???? start_date ? end_date ?????????????? |
| <a href="https://support.office.com/en-us/article/YIELD-function-f5f5ca43-c4bd-434f-8bd2-ed3c9727a4fe" target="_blank">YIELD ??</a> | FunctionResult | ?????????????? |
| <a href="https://support.office.com/en-us/article/YIELDDISC-function-a9dbdbae-7dae-46de-b995-615faffaaed7" target="_blank">YIELDDISC ??</a> | FunctionResult | ???????????????????? |
| <a href="https://support.office.com/en-us/article/YIELDMAT-function-ba7d1809-0d33-4bcb-96c7-6c56ec62ef6f" target="_blank">YIELDMAT ??</a> | FunctionResult | ????????????? |
| <a href="https://support.office.com/en-us/article/ZTEST-function-d633d5a3-2031-4614-a016-92180ad82bee" target="_blank">Z.TEST ??</a> | FunctionResult | ?? z ???????? |

## <a name="see-also"></a>????

- [Excel JavaScript API ????](excel-add-ins-core-concepts.md)
- [Excel JavaScript API ?????](https://github.com/OfficeDev/office-js-docs/tree/ExcelJs_OpenSpec)
- [??????????? Excel ? JavaScript API?](https://dev.office.com/reference/add-ins/excel/functions)