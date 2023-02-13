# playwright-merge-summary-json-reports

![npm (scoped)](https://img.shields.io/npm/v/playwright-merge-summary-json-reports)

This package works in tandem with:

* <https://www.npmjs.com/package/playwright-merge-html-reports>
* <https://www.npmjs.com/package/@butchmayhew/playwright-json-summary-reporter>

The walk through found on <https://playwrightsolutions.com>.

The above package will generate a `summary.json` report after the Playwright run, while this package will combine multiple `summary.json` files into 1 file after a run with the `--shard=x/x` command.

## Install

```bash
npm install playwright-merge-summary-json-reports --save-dev
```

## Usage

Create a new file file in the appropriate directory

```javascript
// lib/metrics/mergeReports.ts

import fs from "fs";
import path from "path";
import { mergeHTMLReports } from "playwright-merge-html-reports";
import { mergeSummary } from "playwright-merge-summary-json-reports";

const reportPathsToMerge = fs
  .readdirSync(process.cwd() + "/playwright-report", { withFileTypes: true })
  .filter((item) => item.isDirectory())
  .map(({ name }) => path.resolve(process.cwd() + "/playwright-report", name));

async function runReport(paths: string[]) {
  // merges the summary.json in each report-x folder and saves a summary.json to root directory
  await mergeSummary(paths);

  // merges html reports and saves to /html-report
  await mergeHTMLReports(paths, {
    outputFolderName: "html-report",
  });
}

runReport(reportPathsToMerge);
```

After a test run, where there is a folder `/playwright-report` with multiple sub-folders `/report-1`, `/report-2/`, etc. Running the command `npx ts-node lib/metrics/mergeReports.ts` will search through the folders for both html and json reports and combine them, and output the html report to `html-report` and the combined summary.json to the main directory.

If you found this helpful feel free to check out <https://playwrightsolutions.com>!
