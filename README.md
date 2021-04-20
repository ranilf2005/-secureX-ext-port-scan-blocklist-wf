# External Port Scan Blocklist SecureX Orchestration Workflow

In this workflow we will reach out to SWC for Inbound Port Scan Alerts. Once we have the alert we will query SWC again for all the observations about that alert. We create a SecureX Orchestration Approval Request and a Webex Teams Alerts message. In the Webex Teams alert message there will be a redirect link to the SWC Alert page, SecureX Threat Response Investigation prepopulated with all the SWC observables, and the SecureX Orchestration Approval request. The next step is to automate adding the attacker IP address to a destination block list in Umbrella.

**Prerequisites:**
1. Cisco Stealthwatch Cloud (SWC) Account (and API key)
2. Cisco SecureX Account (and API key)
3. Cisco Webex Teams Account (and API key)
4. Cisco Umbrella Account (and API key)
    * Block List Object created in Umbrella under **Global Black list**

## Installation Steps
Please follow the below steps exactly to get started!

1. Browse to your SecureX orchestration instance. This wille be a different URL depending on the region your account is in: 

* US: https://securex-ao.us.security.cisco.com/orch-ui/workflows/
* EU: https://securex-ao.eu.security.cisco.com/orch-ui/workflows/
* APJC: https://securex-ao.apjc.security.cisco.com/orch-ui/workflows/

2. Click on **IMPORT** to import the workflow:

![](screenshots/import-workflow.png)

3. Click on **Browse** and copy paste the content of the [secureX-ext-port-scan-blocklist-wf.json](https://raw.githubusercontent.com/emcnicholas/secureX-ext-port-scan-blocklist-wf/main/secureX-ext-port-scan-blocklist-wf.json) file inside of the text window. 

![](screenshots/copy-paste.png)

4. Click on **IMPORT**.

5. Next we will need to fill some API keys and details before we can run this workflow. 

6. First let's update **SWC_Target**. On the main page of Orchestration, go to **Targets**, select **SWC_Target**, and change the host to your SWC base url. Please retrieve your base URL by looking at the URL of your SWC portal. For example if your URL was https://acme.obsrvbl.com/v2/#/settings/site/api-credentials, then you would need **acme.obsrvbl.com/api/v3/** as your base URL target (**SWC_Target**).

![](screenshots/targets_swc.png)

![](screenshots/update_base_url_swc.png)

 7. Next up we will go back into the imported workflow, and we will update the **swc_api_key**. In the **External Port Scan Blocklist Workflow** global workflow properties, scroll down to **Variables**, select the **swc_api_key** variable, and enter your API key in the Value field and save. Please retrieve your SWC API key by loging in to your SWC portal and generate an API key for your use account. To generate an API key, login to your portal and select **Settings > Account Management > API Credentials**, from there, you can generate a unique API key. A key is tied to a specfic user account.

> **Note:** make sure not to select an activity when looking for the global workflow properties.

8. Next we update the **umbrella_api_key** input variable. Select the **umbrella_api_key** variable, and enter your token in the Value field and save.You can generate a CDO API token by logging into your [Cisco Defense Orchestrator portal](https://dashboard.umbrella.com). Go to Admin, APi Key, and create a token. 

9. Now we need to update the **wxt_access_token**. Select the **wxt_access_token** variable, and enter your token in the Value field and save. Please retrieve your Webex key from: [https://developer.webex.com/docs/api/getting-started](https://developer.webex.com/docs/api/getting-started). Please be aware that the personal token from the getting started page only works for 12 hours. Please follow these steps to request a "bot" token: https://developer.webex.com/docs/integrations.

10. Finally we need to update the **wxt_room_id** variable. Select the **wxt_room_id** variable, and enter your Webex Teams room id in the Value field and save. Please retrieve the Webex room ID by creating a new space or finding an existing one via these link: https://developer.webex.com/docs/api/v1/rooms/list-rooms. You can also add the **roomid@webex.bot** bot to the room and it will send you the roomId in a private message and then remove itself from the room.

11. Now it is time to test, click on **RUN** in the top right of your window, and eveyrhting shopuld be working now. If not try troubleshooting by click on the activity that is colored red. 

![](screenshots/run.png)

12. If successful, you should receive a Webex Team Message in the space you configured above similar to the following

![](screenshots/wxt_message2.png)

13. As a final step you could choose to enable to scheduled trigger for this workflow. This is recommended, as the workflow only retrieves the security events of the last 5 minutes. By scheduling it, the Security analysts will be updated every hour for potential new malicious activity. To enable the trigger, click on the trigger in the global workflow properties and uncheck the `DISABLE TRIGGER` checkbox. This can be found in the workflow properties in the right menu pane. 

> **Note:** make sure not to select an activity when looking for the global workflow properties.

## Notes

* Please test this properly before implementing in a production environment. This is a sample workflow!
* The roadmap will include a webhook based trigger, instead of a scheduled run. 

## Contributors (s)

* Ed McNicholas (Cisco)
