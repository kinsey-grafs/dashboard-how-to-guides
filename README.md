# Dashboard How-To Guides

Hands-on Grafana dashboards for training. Every dashboard runs on the built-in **TestData** data source (apart from one optional exception, below), so you don't need to send any real data to Grafana to follow along. Open the JSON files to see how each dashboard is built.

## What's in here

```
dashboards/
├── hands-on-dashboard-labs/        Work through these yourself
│   ├── build-your-own-labels-and-data-links.json
│   └── template-variables-how-to-guide.json
└── example-dashboards/             Finished dashboards to explore and copy from
    ├── dashboard-design-principles/    7 numbered dashboards on dashboard design
    ├── datalinks-and-labeling/         2 dashboards that link to each other
    └── grafana-13-feature-demo.json    Newer dashboard features (needs Prometheus, see below)
```

### Hands-on dashboard labs

| File | What it is |
|---|---|
| `template-variables-how-to-guide.json` | **How-to guide.** Starts with hardcoded panels and walks you through adding a `$server` template variable. About 5 minutes. |
| `build-your-own-labels-and-data-links.json` | **Hands-on lab.** Three exercises: value mappings, thresholds and cell coloring, and data links. Each exercise has a raw "Your turn" panel to edit and a finished "Answer" panel. |

Exercise 3 in the lab links to a dashboard in `example-dashboards/datalinks-and-labeling/`. Import that folder as well, or the data links will land on a "not found" page.

### Example dashboards

| Folder or file | What it is |
|---|---|
| `dashboard-design-principles/` | Seven numbered dashboards on dashboard design. Number 6 ("Production Metrics (DO NOT SHARE)") is a deliberately cluttered example. Despite its name it contains only TestData sample data and placeholder text. |
| `datalinks-and-labeling/service-status-at-a-glance.json` | A status overview whose services link through to the detail dashboard. |
| `datalinks-and-labeling/service-detail-logs.json` | The drill-down target for the data links, including the lab's Exercise 3. |
| `grafana-13-feature-demo.json` | Optional showcase of chained variables, tabs, conditional rendering and responsive layout. See [the Prometheus note](#the-feature-demo-needs-prometheus). |

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

For each file you want:

1. Go to **Dashboards → New → Import**.
2. Upload the JSON file, or paste its contents into the text box, then click **Load**.
3. When asked, pick your **TestData** data source in the data source dropdown. These dashboards were exported with the "Export for sharing externally" option, so they ask you to choose the data source instead of assuming one.
4. Choose a folder and click **Import**.

Keep each dashboard's UID unchanged when you import. The data links between dashboards depend on it:

| Link | From | To (UID) |
|---|---|---|
| Lab Exercise 3 | `build-your-own-labels-and-data-links` | `service-detail-logs` (`jemz7s9`) |
| Status to detail | `service-status-at-a-glance` | `service-detail-logs` (`jemz7s9`) |
| Detail to status | `service-detail-logs` | `service-status-at-a-glance` (`je4s977`) |

Tip: import the two `datalinks-and-labeling` dashboards before you start the lab.

## 3. Work through the labs

### Template variables how-to guide

Open the dashboard and follow the step-by-step panels. You end up with a working `$server` dropdown.

### Labels and data links lab

Open the lab dashboard. The first rows cover the agenda and a short "if you know Splunk, here is the Grafana equivalent" table. Then work through the exercises in order:

1. **Value mappings:** turn a raw availability number into BAU, Warning and Critical.
2. **Thresholds and cell coloring:** show one value two ways in a table.
3. **Data links:** click a service and land on its detail dashboard.

In each exercise, edit the panel marked **🛠 Your turn** by following the steps beside it. When you're done, compare your result with the **✅ Answer** panel.

Tip: edit your own copy. If you get stuck, re-import the JSON file to reset the dashboard.

### The Feature Demo needs Prometheus

`grafana-13-feature-demo.json` shows several newer dashboard features. Most of its panels and variables query Prometheus for Kubernetes metrics such as `container_cpu_usage_seconds_total` and `kube_pod_status_phase`. On a TestData-only setup those panels show **No data**. The importer asks you to pick a data source for Prometheus, and you can point it at any Prometheus instance that has those metrics.

This dashboard is optional. The labs and the other example dashboards don't depend on it.

## How these were made

The dashboards were built in Grafana and exported with the **Export for sharing externally** option (Grafana's **Export** menu on the dashboard). The data source references in the JSON are the `${DS_TESTDATA}` and `${DS_PROMETHEUS}` placeholders, which Grafana fills in at import time.

## Troubleshooting

- **Panels show "Datasource not found":** you skipped the data source dropdown during import, or the TestData data source doesn't exist yet. Add it (step 1) and re-import.
- **"Dashboard with the same UID already exists":** you already have a dashboard with that UID. In the import screen, choose **Import (Overwrite)**, or change the UID. If you change a UID, update the data links that point to it.
- **Data links go to a "not found" page:** import the two `datalinks-and-labeling` dashboards and keep their UIDs.
