---
title: Verify Deployments with Dynatrace
description: This topic shows you how to verify deployments with Dynatrace.
sidebar_position: 7
helpdocs_topic_id: eamwqs2x5a
helpdocs_category_id: 9mefqceij0
helpdocs_is_private: false
helpdocs_is_published: true
---

Harness CV integrates with Dynatrace to:

* Verify that the deployed service is running safely and performing automatic rollbacks.
* Apply machine learning to every deployment to identify and flag anomalies in future deployments.

This topic covers how to add and configure Dynatrace as a Health Source for the Verify step.

## Before You Begin

[Add Dynatrace as a verification provider](https://docs.harness.io/article/g21fb5kfkg-connect-to-monitoring-and-logging-systems#step_add_dynatrace)

## Review: CV Setup Options

To use the Verify step, you will need a Harness Service Reliability Management Monitored Service. In the simplest terms a Harness Monitored Service is a Service and Environment combination, that Harness monitors for:

* Any changes, such as deployments, infrastructure changes, and incidents
* Any health trend deviations using logs and metrics obtained from APM and Logging tools respectively

No matter where you set up the Monitored Service, once it's set up, it's available to both Service Reliability Management and CD modules.

In this topic, we set up the Harness Monitored Service as part of the Verify step setup.

## Step 1: Add Verify Step

There are two ways to add the Verify step:

* **When selecting the stage deployment strategy:**  
The **Verify** step can be enabled in a CD stage the first time you open the **Execution** settings and select the deployment strategy. When you select the deployment strategy you want to use, there is also an **Enable Verification** option. Select the **Enable Verification** option.  
Harness will automatically add the **Verify** step. For example, here is a stage where Canary strategy and the **Enable Verification** option were selected.[![](./static/verify-deployments-with-dynatrace-09.png)](./static/verify-deployments-with-dynatrace-09.png)
* **Add the Verify step to an existing Execution setup:** You can also add the Verify step to the Execution section of a CD stage in a Pipeline you previously created. Simply click **Add Step** after the deployment step, and then select **Verify**.[![](./static/verify-deployments-with-dynatrace-11.png)](./static/verify-deployments-with-dynatrace-11.png)

## Step 2: Enter a Name and Timeout

In **Name**, enter a name for the step.

In **Timeout**, enter a timeout value for the step.

You can use:

* `w` for weeks
* `d` for days
* `h` for hours
* `m` for minutes
* `s` for seconds
* `ms` for milliseconds

The maximum is `53w`.Timeouts can be set at the Pipeline level also.

## Step 3: Select a Continuous Verification Type

In **Continuous Verification Type**, select a type that matches your [deployment strategy](verify-deployments-with-the-verify-step.md#step-3-select-a-continuous-verification-type).

![](./static/verify-deployments-with-dynatrace-13.png)

## Step 4: Create a Monitored Service

In **Monitored Service**, click **Click to autocreate a monitored service**.

Harness automatically creates a Monitored Service using a concatenation of the Service and Environment names. For example, a Service named `todolist` and an Environment named `dev` will result in a Monitored Service named `todolist_dev`.

If the stage Service or Environment settings are [Runtime Inputs](https://ngdocs.harness.io/article/f6yobn7iq0-runtime-inputs), the Monitored Service and Health Sources settings will show up in the **Runtime Input** settings when you run the Pipeline.

## Step 5: Add Health Sources

A Health Source is basically a mapping of a Harness Monitored Service to the Service in a deployment environment monitored by an APM or logging tool.

In **Health Sources**, click **Add**. The **Add New Health Source** settings appear.

![](./static/verify-deployments-with-dynatrace-14.png)

1. In **Select health source type**, select **Dynatrace**.
2. In **Health Source Name**, enter a name for the Health Source. For example Quickstart.
3. Under **Connect Health Source**, click **Select Connector**.
4. In **Connector** settings, you can either choose an existing connector or click **New Connector** to create a new **Connector.**
   
   ![](./static/verify-deployments-with-dynatrace-15.png)

5. After selecting the connector, click **Apply Selected**. The Connector is added to the Health Source.
6. In **Select Feature**, a Dynatrace feature is selected by default.
7. Click **Next**. The **Customize Health Source** settings appear.

The subsequent steps in **Customize Health Source** depend on the Health Source type you selected.

![](./static/verify-deployments-with-dynatrace-16.png)

1. In **Find a Dynatrace service**, enter the name of the desired Dynatrace service.
2. In **Select Metric Packs to be monitored,** you can select **Infrastructure** or **Performance** or both.
3. Click **Add Metric** if you want to add any specific metric to be monitored (optional) or simply click **Submit.**
4. If you click Add Metric, click **Map Metric(s) to Harness Services**.
5. In **Metric Name**, enter the name of the metric.
6. In **Group Name**, enter the group name of the metric.
7. Click **Query Specifications and mapping**.
8. In **Metric**, choose the desired metric from the list.
9. Click **Fetch Records** to retrieve data for the provided query.
10. In **Assign**, choose the services for which you want to apply the metric. Available options are:
	* Continuous Verification
	* Health Score
	* SLI
11. In **Risk Category**, select a risk type.
12. In **Deviation Compared to Baseline**, select one of the options based on the selected risk type.

![](./static/verify-deployments-with-dynatrace-17.png)

Click **Submit**. The Health Source is displayed in the Verify step.

You can add one or more Health Sources for each APM or logging provider.

## Step 6: Select Sensitivity

In **Sensitivity**, select **High**, **Medium**, or **Low** based on the risk level used as failure criteria during the deployment.

## Step 7: Select Duration

Select how long you want Harness to analyze and monitor the logs/APM data points. Harness waits for 2-3 minutes to allow enough time for the data to be sent to the APM/logging tool before it analyzes the data. This wait time is a standard with monitoring tools.

The recommended **Duration** is **10 min** for logging providers and **15 min** for APM and infrastructure providers.### Step 8: Specify Artifact Tag

In **Artifact Tag**, use a [Harness expression](https://newdocs.helpdocs.io/article/lml71vhsim-harness-variables) to reference the artifact in the stage Service settings.

The expression `<+serviceConfig.artifacts.primary.tag>` refers to the primary artifact.

## Option: Advanced Settings

In **Advanced**, you can select the following options:

* [Step Skip Condition Settings](https://docs.harness.io/article/i36ibenkq2-step-skip-condition-settings)
* [Step Failure Strategy Settings](https://docs.harness.io/article/htrur23poj-step-failure-strategy-settings)
* [Select Delegates with Selectors](https://docs.harness.io/article/nnuf8yv13o-select-delegates-with-selectors)

See [Advanced Settings](verify-deployments-with-the-verify-step.md#option-advanced-settings).

## Step 9: Deploy and Review Results

When you are done setting up the **Verify** step, click **Apply Changes**.

Now you can run the Pipeline. Click **Run**.

In **Run Pipeline**, select the tag for the artifact if a tag was not added in the **Artifact Details** settings.

Click **Run Pipeline**.

When the Pipeline is running, click the **Verify** step.

The verification takes a few minutes.

![](./static/verify-deployments-with-dynatrace-18.png)

The risk level might initially display a number of violations, but often changes over the duration.

### Summary

The **Summary** section shows the number of metrics that are in violation.

### Console View

Click **Console View** or simply click **View Details** in **Summary** to take a deeper look at verification.

![](./static/verify-deployments-with-dynatrace-19.png)

If you have more than one Health Source, you can use the **Health Source** dropdown to select each one.
