---
title: Power BI Embedded performance best practices
description: This article provides guidance for embedded analytics best practices
author: rkarlin
ms.author: rkarlin
manager: kfile
ms.reviewer: ''
ms.service: powerbi
ms.subservice: powerbi-embedded
ms.topic: conceptual
ms.date: 12/12/2018
---

# Power BI Embedded performance best practices

This article provides recommendations for faster rendering of reports, dashboards, and tiles in your application

## Embed parameters

Powerbi.embed() method receives few parameters to embed a report, a dashboard, or a tile. These parameters have performance implications.

### Embed URL

Avoid generating the embed URL yourself. Instead, make sure you get the Embed URL by calling the [Get reports](/rest/api/power-bi/reports/getreportsingroup), the [Get dashboards](/rest/api/power-bi/dashboards/getdashboardsingroup), or the [Get tiles](/rest/api/power-bi/dashboards/gettilesingroup) API. We added a new parameter to the URL called **_config_**, used for performance improvements.

### Permissions

Provide **View** permissions if you're not intending to embed a report in **Edit mode**. This way embed code doesn't initialize components, which are used for Edit mode.

### Filters, bookmarks, and slicers

Usually, report visuals are saved with cached data. The cached data is used to give perceived performance. Reports render cached data while queries are executed. If filters, bookmarks or slicers are provided, cached data isn't relevant. Then, the visuals are rendered only after running the visual query.

If you embed reports with the same filters, to avoid passing a list of filters in the load configuration, save the report with filters already applied.

## Preload

Use the *preload* JavaScript API to improve the end-user performance.
Powerbi.preload() downloads javascript, css files, and other artifacts, which is used later to embed in a report.

Call preload if you're not embedding the report immediately. For example, if you embed a report on a button click, it’s better to call preload when the previous page loads. Then when the application user clicks the button, the rendering is faster.

## Measure performance

To measure performance, use:

1. Loaded: time until report is initialized (user sees no spinny).
2. Rendered: time until fully report is rendered using actual data. The rendered event is fired each time the report is re-rendered (that is, after applying filters). To measure a report first, make sure you do the calculations in the first raised event.

Cached data is rendered when available, but we don’t have an event for this data yet.

## Update tools and SDK packages

Keep tools and SDK packages up-to-date.

* Always use the latest version of [Power BI Desktop](https://powerbi.microsoft.com/desktop/).

* Install the latest version of the [Power BI client SDK](https://github.com/Microsoft/PowerBI-JavaScript). We continue to release more enhancements, so make sure to follow up from time to time.

* Packages to install:
    * Npm package: powerbi-client
    * NuGet package: Microsoft.PowerBI.JavaScript

> [!Note]
> Remember that loading time mainly depends on elements relevant to the report and data itself. Such as number of visuals, size of data and complexity of the queries and calculated measures. Please follow [best practices](../power-bi-reports-performance.md) to improve the report’s loading time.

## Next steps

* [Power BI report performance best practices](../power-bi-reports-performance.md)
* [How to troubleshoot Power BI Embedded issues](embedded-troubleshoot.md)
* [Power BI Embedded FAQ](embedded-faq.md)
