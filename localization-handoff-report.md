# <a name='report-top'></a> Localization Handoff Report

## Summary
 Total Files | 2

## File List
 Source File | Status | Details 
 ----------- | ------ | ------- 
 [windows-apps-src\get-started\learn-more.md](https://github.com/Microsoft/windows-apps/blob/43ceaa69d85276274f5a5b2041b0392b7ec9d7f7/windows-apps-src/get-started/learn-more.md) | OutofSyncHandedBackSuccess | [Details](#38bb4cb94347455ba5970fa4227a834fac23253c2676)
 [windows-apps-src\get-started\your-first-app.md](https://github.com/Microsoft/windows-apps/blob/43ceaa69d85276274f5a5b2041b0392b7ec9d7f7/windows-apps-src/get-started/your-first-app.md) | OutofSyncHandedBackSuccess | [Details](#0cf6984f4e93563a4a52ce87d89f19850734c8332681)

## Item Details
##### <a name='38bb4cb94347455ba5970fa4227a834fac23253c2676'></a> Source: [windows-apps-src\get-started\learn-more.md](https://github.com/Microsoft/windows-apps/blob/43ceaa69d85276274f5a5b2041b0392b7ec9d7f7/windows-apps-src/get-started/learn-more.md)
* Status: OutofSyncHandedBackSuccess
* Target File: 
* Handoff File: 
* Handoff Datetime: 0001-01-01 00:00:00
* Handoff Reason: Ignored
* Handoff Error: [handoff_transform_failed](#38bb4cb94347455ba5970fa4227a834fac23253c2676handoff_transform_failed)
* Archive File: 
* Archive Datetime: 0001-01-01 00:00:00
* Handback File: 
* Handback Datetime: 0001-01-01 00:00:00
* Current Target File: [windows-apps-src\get-started\learn-more.md](https://github.com/Microsoft/windows-apps.zh-tw/blob/28d9426b29c49ad4d7d36ad8929a7eab1d0bd985/windows-apps-src/get-started/learn-more.md)
* Current Handback File: [learn-more.83ab93a8bbd1d37afdb4633865d6d04bfbdbe5ee.zh-tw.xlf](https://github.com/Microsoft/WDG.handback/blob/ba466a2470429e980e411fcb9bc1043d0c07ebdd/ol-handback/Microsoft/windows-apps.zh-tw/master/learn-more.83ab93a8bbd1d37afdb4633865d6d04bfbdbe5ee.zh-tw.xlf)
* Current Handback Datetime: 2016-07-20 17:39:23
* [Back to Top](#report-top)

##### <a name='0cf6984f4e93563a4a52ce87d89f19850734c8332681'></a> Source: [windows-apps-src\get-started\your-first-app.md](https://github.com/Microsoft/windows-apps/blob/43ceaa69d85276274f5a5b2041b0392b7ec9d7f7/windows-apps-src/get-started/your-first-app.md)
* Status: OutofSyncHandedBackSuccess
* Target File: 
* Handoff File: [your-first-app.19ca9c0c35670e18229e0b580d71fcd038654594.zh-tw.xlf](https://github.com/Microsoft/WDG.handoff/blob/4b7ee5cc5741e9c31a4364fc5fee07d0454c3dd1/ol-handoff/Microsoft/windows-apps.zh-tw/master/your-first-app.19ca9c0c35670e18229e0b580d71fcd038654594.zh-tw.xlf)
* Handoff Datetime: 2016-07-21 05:57:31
* Handoff Reason: Include
* Archive File: 
* Archive Datetime: 0001-01-01 00:00:00
* Handback File: 
* Handback Datetime: 0001-01-01 00:00:00
* Current Target File: [windows-apps-src\get-started\your-first-app.md](https://github.com/Microsoft/windows-apps.zh-tw/blob/28d9426b29c49ad4d7d36ad8929a7eab1d0bd985/windows-apps-src/get-started/your-first-app.md)
* Current Handback File: [your-first-app.19ca9c0c35670e18229e0b580d71fcd038654594.zh-tw.xlf](https://github.com/Microsoft/WDG.handback/blob/ba466a2470429e980e411fcb9bc1043d0c07ebdd/ol-handback/Microsoft/windows-apps.zh-tw/master/your-first-app.19ca9c0c35670e18229e0b580d71fcd038654594.zh-tw.xlf)
* Current Handback Datetime: 2016-07-20 17:39:23
* [Back to Top](#report-top)


## Error Details
##### <a name='38bb4cb94347455ba5970fa4227a834fac23253c2676handoff_transform_failed'></a> Source: [windows-apps-src\get-started\learn-more.md](#38bb4cb94347455ba5970fa4227a834fac23253c2676)
* Error Code: handoff_transform_failed
* Error Message: Handoff source file: windows-apps-src\get-started\learn-more.md transformed failed.
* Retriable: False
* Error Details: {"internal_error_code":"handoff_transform_failed","internal_error_message":"Handoff source file: windows-apps-src\\get-started\\learn-more.md transformed failed.","internal_error_retriable":false,"exception_message":"","exception_type":"System.Net.Http.HttpRequestException","stack_trace":"   at Microsoft.OpenLocalization.Transformer.MarkdownJavascriptTransformer.MarkdownToXliffCore(Stream sourceStream, Stream xliffStream, Stream skeletonStream, String srcLocale, String targetLocale, IReadOnlyDictionary`2 transformerOptions) in C:\\Jenkins\\workspace\\OpenLocalization-Prod\\src\\OpenLocalization.Transformer.Core\\Transformer\\MarkdownJavascriptTransformer.cs:line 43\r\n   at Microsoft.OpenLocalization.Transformer.TransformerClient.MarkdownToXliff(Stream sourceStream, Stream xliffStream, Stream skeletonStream, String srcLocale, String targetLocale, String xliffVersion, IReadOnlyDictionary`2 options) in C:\\Jenkins\\workspace\\OpenLocalization-Prod\\src\\OpenLocalization.Transformer.Core\\TransformerClient.cs:line 70\r\n   at Microsoft.OpenLocalization.Helper.XliffTransformUtil.MarkdownToXliff(String mdfile, String xliffFile, String skeletonFile, String sourceLocale, String targetLocale, String xliffVersion, Boolean useJavascriptTransformer, IReadOnlyDictionary`2 transformerOptions) in C:\\Jenkins\\workspace\\OpenLocalization-Prod\\src\\OpenLocalization\\Helper\\XliffTransformUtil.cs:line 55\r\n   at Microsoft.OpenLocalization.Localization.LocalizationCore.<>c__DisplayClass12_0.<CreateHandoffFiles>b__0(Tuple`3 handoff) in C:\\Jenkins\\workspace\\OpenLocalization-Prod\\src\\OpenLocalization\\Localization\\HandoffCore.cs:line 223","extended_information":null}


Generated by OpenLocalization.
