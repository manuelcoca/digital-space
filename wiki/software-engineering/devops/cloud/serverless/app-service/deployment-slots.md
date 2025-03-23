## Benefits

-   Validate app changes in a staging deployment slot before swapping it with the production slot
-   Deploying an app to a slot first and swapping it into production makes sure that all instances of the slot are warmed up before being swapped into production. This eliminates downtime when you deploy your app. The traffic redirection is seamless, and no requests are dropped because of swap operations
-   After a swap, the slot with previously staged app now has the previous production app. If the changes swapped into the production slot aren't as you expect, you can perform the same swap immediately to get your "last known good site" back

## Swap settings

| Settings that are swapped                                           | Settings that aren't swapped                           |
| ------------------------------------------------------------------- | ------------------------------------------------------ |
| General settings, such as framework version, 32/64-bit, web sockets | Publishing endpoints                                   |
| App settings (can be configured to stick to a slot)                 | Custom domain names                                    |
| Connection strings (can be configured to stick to a slot)           | Non-public certificates and TLS/SSL settings           |
| Handler mappings                                                    | Scale settings                                         |
| Public certificates                                                 | WebJobs schedulers                                     |
| WebJobs content                                                     | IP restrictions                                        |
| Hybrid connections \*                                               | Always On                                              |
| Azure Content Delivery Network \*                                   | Diagnostic log settings                                |
| Service endpoints \*                                                | Cross-origin resource sharing (CORS)                   |
| Path mappings                                                       | Virtual network integration                            |
|                                                                     | Managed identities                                     |
|                                                                     | Settings that end with the suffix `_EXTENSION_VERSION` |

## Manually swapping deployment slots <a href="#manually-swapping-deployment-slots" id="manually-swapping-deployment-slots"></a>

To swap deployment slots:

1. Go to your app's **Deployment slots** page and select **Swap**. The **Swap** dialog box shows settings in the selected source and target slots that will be changed.
2. Select the desired **Source** and **Target** slots. Usually, the target is the production slot. Also, select the **Source Changes**and **Target Changes** tabs and verify that the configuration changes are expected. When you're finished, you can swap the slots immediately by selecting **Swap**.

    To see how your target slot would run with the new settings before the swap actually happens, don't select Swap, but follow the instructions in _Swap with preview_ below.

3. When you're finished, close the dialog box by selecting Close.

## Swap with preview (multi-phase swap) <a href="#swap-with-preview-multi-phase-swap" id="swap-with-preview-multi-phase-swap"></a>

Before you swap into production as the target slot, validate that the app runs with the swapped settings. The source slot is also warmed up before the swap completion, which is desirable for mission-critical applications.

When you perform a swap with preview, App Service performs the same swap operation but pauses after the first step. You can then verify the result on the staging slot before completing the swap.

If you cancel the swap, App Service reapplies configuration elements to the source slot.

To swap with preview:

1. Follow the steps above in Swap deployment slots but select **Perform swap with preview**. The dialog box shows you how the configuration in the source slot changes in phase 1, and how the source and target slot change in phase 2.
2. When you're ready to start the swap, select **Start Swap**.

    When phase 1 finishes, you're notified in the dialog box. Preview the swap in the source slot by going to `https://<app_name>-<source-slot-name>.azurewebsites.net`.

3. When you're ready to complete the pending swap, select **Complete Swap** in **Swap action** and select **Complete Swap**.

    To cancel a pending swap, select **Cancel Swap** instead.

4. When you're finished, close the dialog box by selecting **Close**.

## Configure auto swap

Auto swap streamlines Azure DevOps Services scenarios where you want to deploy your app continuously with zero cold starts and zero downtime for customers of the app. When auto swap is enabled from a slot into production, every time you push your code changes to that slot, App Service automatically swaps the app into production after it's warmed up in the source slot.

Note

Auto swap isn't currently supported in web apps on Linux and Web App for Containers.

To configure auto swap:

1. Go to your app's resource page and select the deployment slot you want to configure to auto swap. The setting is on the **Configuration > General settings** page.
2. Set **Auto swap enabled** to **On**. Then select the desired target slot for Auto swap deployment slot, and select **Save** on the command bar.
3. Execute a code push to the source slot. Auto swap happens after a short time, and the update is reflected at your target slot's URL.

## Roll back and monitor a swap

If any errors occur in the target slot (for example, the production slot) after a slot swap, restore the slots to their pre-swap states by swapping the same two slots immediately.

If the swap operation takes a long time to complete, you can get information on the swap operation in the activity log.

On your app's resource page in the portal, in the left pane, select **Activity log**.

A swap operation appears in the log query as `Swap Web App Slots`. You can expand it and select one of the suboperations or errors to see the details.

## Route traffic to another Slot

By default, all client requests to the app's production URL (`http://<app_name>.azurewebsites.net`) are routed to the production slot. You can route a portion of the traffic to another slot. This feature is useful if you need user feedback for a new update, but you're not ready to release it to production.

To route production traffic automatically:

1. Go to your app's resource page and select **Deployment slots**.
2. In the **Traffic %** column of the slot you want to route to, specify a percentage (between 0 and 100) to represent the amount of total traffic you want to route. Select **Save**.
