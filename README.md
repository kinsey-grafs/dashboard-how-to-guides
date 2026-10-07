# Dashboard How-To Guides

Hands-on Grafana dashboards for training. Every dashboard runs on the built-in **TestData** data source (apart from one optional exception, below), so you don't need to send any real data to Grafana to follow along. Open the JSON files to see how each dashboard is built.

## What's in here

| File | What it is | Data source |
|---|---|---|
| [`dashboards/template-variables-how-to-guide.json`](dashboards/template-variables-how-to-guide.json) | **How-to guide.** Starts with hardcoded panels and walks you through adding a `$server` template variable. | TestData |
| [`dashboards/lab-build-your-own-labels-and-data-links.json`](dashboards/lab-build-your-own-labels-and-data-links.json) | **Hands-on lab.** Three exercises: value mappings, thresholds and cell coloring, and data links. Each exercise has a raw "Your turn" panel to edit and a finished "Answer" panel. | TestData |
| [`dashboards/lab-support-service-detail-logs.json`](dashboards/lab-support-service-detail-logs.json) | **Lab support dashboard.** The drill-down target for Exercise 3 in the lab above. Import it too, or the data links will land on a "not found" page. | TestData |
| [`dashboards/grafana-13-feature-demo.json`](dashboards/grafana-13-feature-demo.json) | **Optional feature showcase.** Chained variables, tabs, conditional rendering and responsive layout. | TestData **and Prometheus** (see [below](#the-feature-demo-needs-prometheus)) |

## Before you start

You need:

- A Grafana instance you can edit. A free [Grafana Cloud](https://grafana.com/auth/sign-up/create-user) account works.
- Permission to add data sources and import dashboards (Admin or Editor, depending on your setup).

## 1. Add the TestData data source

1. In Grafana, go to **Connections → Data sources → Add new data source**.
2. Search for **TestData** and select it.
3. Leave the defaults and click **Save & test**.

TestData generates random walks, CSV content and other sample series. Nothing is stored, and no external system is involved.

## 2. Import the dashboards

For each file in [`dashboards/`](dashboards/):

1. Go to **Dashboards → New → Import**.
2. Upload the JSON file, or paste its contents into the text box, then click **Load**.
3. When asked, pick your **TestData** data source in the data source dropdown. These dashboards were exported with the "Export for sharing externally" option, so they ask you to choose the data source instead of assuming one.
4. Choose a folder and click **Import**.

Import `lab-support-service-detail-logs.json` along with the lab. Its dashboard UID (`jemz7s9`) matches the link the lab uses in Exercise 3, so keep the UID unchanged when you import.

## 3. Work through the dashboards

### Template variables how-to guide

Open the dashboard and follow the step-by-step panels. You end up with a working `$server` dropdown. It takes about five minutes.

### Hands-on lab: labels and data links

Open the lab dashboard. The first rows cover the agenda and a short "if you know Splunk, here is the Grafana equivalent" table. Then work through the exercises in order:

1. **Value mappings:** turn a raw availability number into BAU, Warning and Critical.
2. **Thresholds and cell coloring:** show one value two ways in a table.
3. **Data links:** click a service and land on its detail dashboard.

In each exercise, edit the panel marked **🛠 Your turn** by following the steps beside it. When you're done, compare your result with the **✅ Answer** panel.

Tip: edit your own copy. If you get stuck, re-import the JSON file to reset the dashboard.

### The Feature Demo needs Prometheus

`grafana-13-feature-demo.json` shows several newer dashboard features. Most of its panels and variables query Prometheus for Kubernetes metrics such as `container_cpu_usage_seconds_total` and `kube_pod_status_phase`. On a TestData-only setup those panels show **No data**. The importer asks you to pick a data source for Prometheus, and you can point it at any Prometheus instance that has those metrics.

This dashboard is optional. The how-to guide and the lab don't depend on it.

## How these were made

The dashboards were built in Grafana and exported with the **Export for sharing externally** option (Grafana's **Export** menu on the dashboard). The data source references in the JSON are the `${DS_TESTDATA}` and `${DS_PROMETHEUS}` placeholders, which Grafana fills in at import time.

## Troubleshooting

- **Panels show "Datasource not found":** you skipped the data source dropdown during import, or the TestData data source doesn't exist yet. Add it (step 1) and re-import.
- **"Dashboard with the same UID already exists":** you already have a dashboard with that UID. In the import screen, choose **Import (Overwrite)**, or change the UID. If you change the lab support dashboard's UID, update the link in the lab's Exercise 3 as well.
- **Data links go to a "not found" page:** import `lab-support-service-detail-logs.json` and keep its UID `jemz7s9`.
