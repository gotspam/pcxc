Lab 1: Deploying and Managing F5 Distributed Cloud Web Application Firewall Configuration
=========================================================================================

Task 1: Configure Load Balancer and Origin Pool
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The following steps will allow you to deploy and advertise a globally available application.  These
steps will define an application, register its DNS and assign a target as an origin.

+----------------------------------------------------------------------------------------------+
| 1. Following the **Introduction** section instructions, you should now be in the **Web**     |
|                                                                                              |
|    **App & API Protection** configuration window. If for some reason you are not in the      |
|                                                                                              |
|    **Web App & API Protection** window, use the **Select Service** in the left-hand          |
|                                                                                              |
|    navigation, and click **Web App & API Protection** as shown in the *Introduction Section* |
|                                                                                              |
| 2. In the left-hand navigation expand **Manage** and click **Load Balancers > HTTP Load**    |
|                                                                                              |
|    **Balancers**                                                                             |
|                                                                                              |
| 3. In the resulting screen click the **Add HTTP Load Balancer** in the graphic as shown.     |
+----------------------------------------------------------------------------------------------+
| |lab001|                                                                                     |
|                                                                                              |
| |lab002|                                                                                     |
+----------------------------------------------------------------------------------------------+

Task 2: Configure WAF Policy on the Load Balancer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The following steps will guide you through adding a Web Application Firewall (WAF) Policy.
These steps will demonstrate various aspects of the configuration.

+----------------------------------------------------------------------------------------------+
| 1. Continuing in the **HTTP Load Balancer** section, on the left-hand menu click on the      |
|                                                                                              |
|    **Web Application Firewall (WAF)** drop down and select **Enable**.                       |
+----------------------------------------------------------------------------------------------+
| |lab011|                                                                                     |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 2. In the resulting **Enable** field drop down, select **Add Item**.                         |
+----------------------------------------------------------------------------------------------+
| |lab012|                                                                                     |
+----------------------------------------------------------------------------------------------+

.. note::
   *The "shared/base-appfw" policy is in the "shared namespace" which can be applied to* 
   *multiple Load Balancer configurations across namespaces, reducing policy sprawl.* 

+----------------------------------------------------------------------------------------------+
| 3. In the new window's **Metadata** section enter **<namespace>-appfw** for the              |
|    **Name**.                                                                                 |
|                                                                                              |
| 4. Under **Enforcement Mode**, change the mode to **Blocking**.                              |
|                                                                                              |
| 5. In the **Detection Settings** section, click the **Security Policy** dropdown.            |
|                                                                                              |
| 6. Select **Custom** from the dropdown menu. Additional configurations will become available.|
+----------------------------------------------------------------------------------------------+
| |lab013|                                                                                     |
|                                                                                              |
| |lab014|                                                                                     |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 7. In the expanded configuration, in the **Attack Signatures** section use the dropdown for  |
|                                                                                              |
|    **Signature Selection by Accuracy** and select **High, Medium, and Low**.                 |
+----------------------------------------------------------------------------------------------+
| |lab015|                                                                                     |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 8. Leaving all other values as default, scroll to the bottom and click **Continue**.         |
+----------------------------------------------------------------------------------------------+
| |lab016|                                                                                     |
+----------------------------------------------------------------------------------------------+

.. note::
   *Automatic Attack Signatures Tuning is enabled which engages an automatic False Positive*
   *suppression engine.  Any suppressed events are logged within Security Events.*

.. note::
   *Attack Signatures Staging is disabled.  This can be enabled should an Application*
   *Firewall with new or updated attack signatures be staged (monitored) for a period of*
   *prior to enforcement (blocking).*

+----------------------------------------------------------------------------------------------+
| 9. Returning to the **HTTP Load Balancer** window, scroll to (or click in the left-hand      |
|                                                                                              |
|    navigation) to the **Other Settings** section and note the **VIP Advertisement** setting. |
|                                                                                              |
| 10. Click **Save and Exit** at the bottom of the **HTTP Load Balancer** configuration screen.|
+----------------------------------------------------------------------------------------------+
| |lab017|                                                                                     |
+----------------------------------------------------------------------------------------------+

.. note::
   *The above selection controls how/where the application is advertised. The "Internet"*
   *setting means that this application will be advertised globally from the F5*
   *Distributed Cloud Global Network utilizing Anycast.*

+----------------------------------------------------------------------------------------------+
| 10. In the **HTTP Load Balancers** window, note the application hostname under the           |
|                                                                                              |
|     **Domains** column *(This was done in Task1: Step 4)*.                                   |
|                                                                                              |
| 11. Click the **Action** dots, and then in the subsequent menu **Manage Configuration**.     |
+----------------------------------------------------------------------------------------------+
| |lab018|                                                                                     |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 12. Click **DNS Information** in the left-hand navigation.                                   |
|                                                                                              |
|     The value for a CNAME is listed under **Host Name**. The associated "Default/Tenant IP"  |
|                                                                                              |
|     is also shown under **IP Address**. The "Default/Tenant IP" is uniquely assigned to each |
|                                                                                              |
|     F5 Distributed Cloud Tenant. Additional Public IPs can be added to the Tenant.           |
+----------------------------------------------------------------------------------------------+
| |lab019|                                                                                     |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 13. Click **JSON** in the horizontal navigation at the top-left of the screen.               |
|                                                                                              |
|     The JSON payload (or YAML format, from dropdown) provides for the entire Load Balancer   |
|                                                                                              |
|     configuration.  This can be used for backup or subsequent CI/CD automation operations.   |
+----------------------------------------------------------------------------------------------+
| |lab020|                                                                                     |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 14. Click **Documentation** in the horizontal navigation at the top of the screen.           |
|                                                                                              |
|     The Documentation screen provides details on the F5 Distributed Cloud Console API.       |
|                                                                                              |
|     All operations in the F5 Distributed Cloud Platform are API-first. This includes all GUI |
|                                                                                              |
|     actions and associated audit logging.                                                    |
|                                                                                              |
| 15. Click **Cancel and Exit** to return to the **HTTP Load Balancers** screen.               |
+----------------------------------------------------------------------------------------------+
| |lab021|                                                                                     |
+----------------------------------------------------------------------------------------------+

Task 3: Testing the WAF Policy & Reviewing Event Data  
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You will now perform basic testing of the Web Application Firewall (WAF) Policy. You will also 
review the generated event data to make additional configuration changes.

+----------------------------------------------------------------------------------------------+
| 1. Open another tab in your browser (Chrome shown), navigate to the newly configured Load    |
|                                                                                              |
|    Balancer configuration: **http://<namespace>.lab-sec.f5demos.com**, to confirm it is      |
|                                                                                              |
|    functional.                                                                               |
|                                                                                              |
| 2. Using some of the sample attacks below, add the URI path & variables to your application  |
|                                                                                              |
|    to generate security event data.                                                          |
|                                                                                              |
|    * /?cmd=cat%20/etc/passwd                                                                 |
|    * /product?id=4%20OR%201=1                                                                |
|    * /../../../../etc/shadow                                                                 |
|    * /cart?search=aaa'><script>prompt('Please+enter+your+password');</script>                |
|                                                                                              |
| 3. In the resulting block screens, note the **URL** and the **Support ID**. (copy and paste  |
|                                                                                              |
|    to a notepad or note resource).                                                           |
+----------------------------------------------------------------------------------------------+
| |lab022|                                                                                     |
|                                                                                              |
| |lab023|                                                                                     |
|                                                                                              |
| |lab024|                                                                                     |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 4. Returning to the F5 Distributed Cloud Console, use the left-hand menu to select           |
|                                                                                              |
|    **Overview > Dashboard > Performance Dashboard** section.  This dashboard will provide a  |
|                                                                                              |
|    summary view for all of the configured Load Balancers.                                    |
+----------------------------------------------------------------------------------------------+
| |lab025|                                                                                     |
+----------------------------------------------------------------------------------------------+

.. note::
   *As you have not run many requests, summary analytics may not be available in the*
   *dashboard view yet.*

+----------------------------------------------------------------------------------------------+
| 5. Scroll to the bottom and select your load balancer.                                       |
+----------------------------------------------------------------------------------------------+
| |lab026|                                                                                     |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 6. From the **Performance Dashboard** view, using the horizontal navigation, click           |
|                                                                                              |
|    **Requests**.                                                                             |
|                                                                                              |
| 7. Change the viewable time period from 5 minutes (default) to **1 hour** by selecting the   |
|                                                                                              |
|    dropdown shown, click **Last 1 hour** then clicking **Apply**.                            |
+----------------------------------------------------------------------------------------------+
| |lab027|                                                                                     |
+----------------------------------------------------------------------------------------------+

.. note::
   *Security Event data may take 15-20 seconds to populate in the Console. Please force a*
   *refresh using the Refresh icon next to the Time Period selection in step 6.*

+----------------------------------------------------------------------------------------------+
| 8. Expand one of the requests and note the **Information** tab link. This summarizes request |
|                                                                                              |
|    details and provides request duration timing.                                             |
+----------------------------------------------------------------------------------------------+
| |lab028|                                                                                     |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 9. Click on the **JSON** link to get more data about the request.                            |
|                                                                                              |
| 10. Click **Add Filter** as shown to see how you can filter by key identifiers.              |
+----------------------------------------------------------------------------------------------+
| |lab029|                                                                                     |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 11. The resulting **Search** input field and listed **Keys**, can be used to filter requests |
|                                                                                              |
|     in this view.                                                                            |
+----------------------------------------------------------------------------------------------+
| |lab030|                                                                                     |
+----------------------------------------------------------------------------------------------+

.. note::
   *The available Key list to search is dynamically updated based on the requests in the*
   *selected time view.* 

+----------------------------------------------------------------------------------------------+
| 12. Closing the filters view, note the available **Quick Filters** for Response Codes which  |
|                                                                                              |
|     allows quickly filtering the requests by toggling **on** or **off** each response code   |
|                                                                                              |
|     category.                                                                                |
|                                                                                              |
| 13. Click the **Forensics** tab on the right side of the view as shown.                      |
+----------------------------------------------------------------------------------------------+
| |lab031|                                                                                     |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 14. The *Forensics* Filter  provides summarized top categories which provides quicker        |
|                                                                                              |
|     analysis of the request log data.  Collapse the **Forensics** view when done using the   |
|                                                                                              |
|     indicated arrow.                                                                         |
+----------------------------------------------------------------------------------------------+
| |lab032|                                                                                     |
+----------------------------------------------------------------------------------------------+

.. note::
   *Individual forensic categories can be changed using the noted pencil icon to surface*
   *additional top data details.*

+----------------------------------------------------------------------------------------------+
| 15. Using the left-hand navigation, click **Dashboards** and then select **Security**        |
|                                                                                              |
|     **Dashboard**.                                                                           |
+----------------------------------------------------------------------------------------------+
| |lab033|                                                                                     |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 16. Review the **Security Dashboard** display (you may have limited data) .                  |
+----------------------------------------------------------------------------------------------+
| |lab034|                                                                                     |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 17. Scroll to **Load Balancers** section and click the **<namespace>-lb** object.            |
+----------------------------------------------------------------------------------------------+
| |lab035|                                                                                     |
+----------------------------------------------------------------------------------------------+

.. note::
   *This is a multi-application view. Here you could get the summary security status of*
   *each application (iw Threat Level, WAF Mode, etc)* and then click into one for more*
   *specific details.*

+----------------------------------------------------------------------------------------------+
| 18. From the **Security Dashboard** view, using the horizontal navigation, click **Security**|
|                                                                                              |
|     **Analytics**.                                                                           |
+----------------------------------------------------------------------------------------------+
| |lab036|                                                                                     |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 19. Expand your latest security event as shown.                                              |
+----------------------------------------------------------------------------------------------+
| |lab037|                                                                                     |
+----------------------------------------------------------------------------------------------+

.. note::
   *If you lost your 1 Hour Filter, re-apply using Task 3: Step 6*

+----------------------------------------------------------------------------------------------+
| 20. Note the summary detail provided **Information** link and identify the **Request ID**    |
|                                                                                              |
|     which is synonymous with **Support ID** (filterable) from the Security Event Block Page. |
+----------------------------------------------------------------------------------------------+
| |lab038|                                                                                     |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 21. Scroll to the bottom of the information screen to see specific signatures detected and   |
|                                                                                              |
|     actions taken during the security event.                                                 |
|                                                                                              |
| 22. Next, click on the **Add Filter** link just under the **Security Analytics** title near  |
|                                                                                              |
|     the top of the **Security Analytics** window.                                            |
+----------------------------------------------------------------------------------------------+
| |lab039|                                                                                     |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 23. Type **req** in the open dialogue window and select **req_id** from the dropdown.        |
|                                                                                              |
| 24. Next, select **In** from the **Select Operator** dropdown.                               |
|                                                                                              |
| 25. Finally, select/assign a value that matches one of your copied **Support IDs** from      |
|                                                                                              |
|     Task 3, Step 2 as shown.  You can also optionally just paste the Support ID in the       |
|                                                                                              |
|     value field and click **Apply**.                                                         |
+----------------------------------------------------------------------------------------------+
| |lab040|                                                                                     |
|                                                                                              |
| |lab041|                                                                                     |
|                                                                                              |
| |lab042|                                                                                     |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 26. You should now be filtered to a single "Security Event", as shown with your selected     |
|                                                                                              |
|     filter. You can expand and review the request as desired using the **arrow** icon.       |
|                                                                                              |
| 27. Under the **Actions** column, click on the three Action dots (Scroll to right).          |
|                                                                                              |
| 28. Select **Create WAF Exclusion rule** from the dropdown that appears.                     |
+----------------------------------------------------------------------------------------------+
| |lab043|                                                                                     |
+----------------------------------------------------------------------------------------------+

.. note::
   *Adding requestor/client to "Blocked or Trusted Clients" is also available.* 

+----------------------------------------------------------------------------------------------+
| 29. In the subsequent **Simple WAF Exclusion Rule** window, review the settings (which are   |
|                                                                                              |
|     editable) by scrolling through the window.  The values have been auto-populated based on |
|                                                                                              |
|     the selected event to be excluded. Note the various sections and possible controls.      |
|                                                                                              |
| 30. In the **Expiration  Timestamp** field enter a timestamp 10 minutes from now at which    |
|                                                                                              |
|     the exclusion should expire. (helpful when testing/validating). the format should be as  |
|                                                                                              |
|     shown *YYYY-MM-DD HH:MM:SS+00:00 (2023-05-30T01:21:00+00:00)*. **HH** must be in 24 hour |
|                                                                                              |
|     format (there is no AM/PM).                                                              |
|                                                                                              |
| 31. Click **Apply** when complete.                                                           |
+----------------------------------------------------------------------------------------------+
| |lab044|                                                                                     |
|                                                                                              |
| |lab045|                                                                                     |
|                                                                                              |
| |lab046|                                                                                     |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 32. Click **Apply** on the **WAF Exclusion Rules** summary screen.                           |
|                                                                                              |
| 33. Click on **Security Configuration** in the left-hand navigation and note the added       |
|                                                                                              |
|     **WAF Exclusion Rules** configuration.                                                   |
|                                                                                              |
| 34. Scroll to the bottom of the **HTTP Load Balancer** configuration window and click the    |
|                                                                                              |
|     **Save and Exit** button.                                                                |
|                                                                                              |
| .. note::                                                                                    |
|    *Rerunning the attack you just excluded, you will see that it is no longer blocked*.      |
+----------------------------------------------------------------------------------------------+
| |lab047|                                                                                     |
|                                                                                              |
| |lab048|                                                                                     |
|                                                                                              |
| |lab049|                                                                                     |
+----------------------------------------------------------------------------------------------+

Task 4: Understanding Exclusions and Customizing WAF Policy  
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In this task you will come to understand how exclusions are applied. You will also further  
customize the WAF policy just built.

+----------------------------------------------------------------------------------------------+
| 1. In the **HTTP Load Balancers** window **(Manage > Load Balancers > HTTP Load Balancers)** |
|                                                                                              |
|    Click on the three action dots in the **Actions** column then **Manage Configuration**    |
|                                                                                              |
|    from the dropdown menu.                                                                   |
+----------------------------------------------------------------------------------------------+
| |lab050|                                                                                     |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 2. Click on the **JSON** tab in the horizontal navigation as shown and scroll to find the    |
|                                                                                              |
|    **waf_exclusion_rule** section. Observe that the exclusion rule is associated with the    |
|                                                                                              |
|    Load Balancer configuration and not the WAF Policy.                                       |
|                                                                                              |
| .. note::                                                                                    |
|    *This allows for policy reuse and reduces the need for specific application WAF Policies*.|
|                                                                                              |
| 3. Click on the **Cancel and Exit** to return to the prior window.                           |
+----------------------------------------------------------------------------------------------+
| |lab051|                                                                                     |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 4. In the left-hand navigation menu, navigate  to **Manage** section and click the **App**   |
|                                                                                              |
|    **Firewall** link.                                                                        |
|                                                                                              |
| 5. On your App Firewall policy **<namespace>-appfw**, click the three dots in the **Actions**|
|                                                                                              |
|    column and then click **Manage Configuration**.                                           |
+----------------------------------------------------------------------------------------------+
| |lab052|                                                                                     |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 6. Click **Edit Configuration** in the top right corner.                                     |
|                                                                                              |
| 7. Use the left-hand navigation and click on **Advanced Configuration**.                     |
+----------------------------------------------------------------------------------------------+
| |lab053|                                                                                     |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 8. Click the drop down menu for **Blocking Response Page** in the **Advanced Configuration** |
|                                                                                              |
|    section.                                                                                  |
+----------------------------------------------------------------------------------------------+
| |lab054|                                                                                     |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 9. Click the dropdown on **Blocking Response Page** and select **Custom** from the dropdown. |
+----------------------------------------------------------------------------------------------+
| |lab055|                                                                                     |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| 10. In the **Blocking Response Page Body** replace the existing text with the text provided  |
|                                                                                              |
|     below. Click **Save and Exit** when completed.                                           |
+----------------------------------------------------------------------------------------------+
| |lab056|                                                                                     |
+----------------------------------------------------------------------------------------------+

Sample Blocking Response Page to be copied::

  <style>body { font-family: Source Sans Pro, sans-serif; }</style>
  <html style="margin: 0;"><head><title>Rejected Request</title></head>
  <body style="margin : 0;">
  <div style="background-color: #046b99; height: 40px; width: 100%;"></div>
  <div style="min-height: 100px; background-color: white; text-align: center;"></div>
  <div style="background-color: #fdb81e; height: 5px; width: 100%;"></div>
  <div id="main-content" style="width: 100%; ">
  <table width="100%"><tr><td style="text-align: center;">
  <div style="margin-left: 50px;">
  <div style="margin-bottom: 35px;"><br/>
  <span style="font-size: 40pt; color: #046b99;">Rejected Request</span>
  </div><div style="font-size: 14pt;">
  <p>The requested URL was rejected. Please consult with your administrator.</p>
  <p>Your Support ID is: <span style="color:red; font-weight:bold">{{request_id}}</span></p>
  <p><a href="javascript:history.back()">[Go Back]</a></p>
  </div></div></td></tr></table></div>
  <div style="background-color: #222222; position: fixed; bottom: 0px; height: 40px; width: 100%; text-align: center;"></div>
  </body></html>

+----------------------------------------------------------------------------------------------+
| 11. You can rerun an attack from Task 3: Step 2 to see the new custom block page.            |
+----------------------------------------------------------------------------------------------+
| |lab057|                                                                                     |
+----------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------+
| **End of Lab 1:**  This concludes Lab 1, feel free to review and test the configuration.     |
|                                                                                              |
| A brief presentation will be shared prior to the beginning of Lab 2.                         |
+----------------------------------------------------------------------------------------------+
| |labend|                                                                                     |
+----------------------------------------------------------------------------------------------+

.. |lab001| image:: /assets/lab1-001.png
   :width: 800px
.. |lab002| image:: /assets/lab1-002.png
   :width: 800px
.. |lab003| image:: /assets/lab1-003.png
   :width: 800px
.. |lab004| image:: /assets/lab1-004.png
   :width: 800px
.. |lab005| image:: /assets/lab1-005.png
   :width: 800px
.. |lab006| image:: /assets/lab1-006.png
   :width: 800px
.. |lab007| image:: /assets/lab1-007.png
   :width: 800px
.. |lab008| image:: /assets/lab1-008.png
   :width: 800px
.. |lab009| image:: /assets/lab1-009.png
   :width: 800px
.. |lab010| image:: /assets/lab1-010.png
   :width: 800px
.. |lab011| image:: /assets/lab1-011.png
   :width: 800px
.. |lab012| image:: /assets/lab1-012.png
   :width: 800px
.. |lab013| image:: /assets/lab1-013.png
   :width: 800px
.. |lab014| image:: /assets/lab1-014.png
   :width: 800px
.. |lab015| image:: /assets/lab1-015.png
   :width: 800px
.. |lab016| image:: /assets/lab1-016.png
   :width: 800px
.. |lab017| image:: /assets/lab1-017.png
   :width: 800px
.. |lab018| image:: /assets/lab1-018.png
   :width: 800px
.. |lab019| image:: /assets/lab1-019.png
   :width: 800px
.. |lab020| image:: /assets/lab1-020.png
   :width: 800px
.. |lab021| image:: /assets/lab1-021.png
   :width: 800px
.. |lab022| image:: /assets/lab1-022.png
   :width: 800px
.. |lab023| image:: /assets/lab1-023.png
   :width: 800px
.. |lab024| image:: /assets/lab1-024.png
   :width: 800px
.. |lab025| image:: /assets/lab1-025.png
   :width: 800px
.. |lab026| image:: /assets/lab1-026.png
   :width: 800px
.. |lab027| image:: /assets/lab1-027.png
   :width: 800px
.. |lab028| image:: /assets/lab1-028.png
   :width: 800px
.. |lab029| image:: /assets/lab1-029.png
   :width: 800px
.. |lab030| image:: /assets/lab1-030.png
   :width: 800px
.. |lab031| image:: /assets/lab1-031.png
   :width: 800px
.. |lab032| image:: /assets/lab1-032.png
   :width: 800px
.. |lab033| image:: /assets/lab1-033.png
   :width: 800px
.. |lab034| image:: /assets/lab1-034.png
   :width: 800px
.. |lab035| image:: /assets/lab1-035.png
   :width: 800px
.. |lab036| image:: /assets/lab1-036.png
   :width: 800px
.. |lab037| image:: /assets/lab1-037.png
   :width: 800px
.. |lab038| image:: /assets/lab1-038.png
   :width: 800px
.. |lab039| image:: /assets/lab1-039.png
   :width: 800px
.. |lab040| image:: /assets/lab1-040.png
   :width: 800px
.. |lab041| image:: /assets/lab1-041.png
   :width: 800px
.. |lab042| image:: /assets/lab1-042.png
   :width: 800px
.. |lab043| image:: /assets/lab1-043.png
   :width: 800px
.. |lab044| image:: /assets/lab1-044.png
   :width: 800px
.. |lab045| image:: /assets/lab1-045.png
   :width: 800px
.. |lab046| image:: /assets/lab1-046.png
   :width: 800px
.. |lab047| image:: /assets/lab1-047.png
   :width: 800px
.. |lab048| image:: /assets/lab1-048.png
   :width: 800px
.. |lab049| image:: /assets/lab1-049.png
   :width: 800px
.. |lab050| image:: /assets/lab1-050.png
   :width: 800px
.. |lab051| image:: /assets/lab1-051.png
   :width: 800px
.. |lab052| image:: /assets/lab1-052.png
   :width: 800px
.. |lab053| image:: /assets/lab1-053.png
   :width: 800px
.. |lab054| image:: /assets/lab1-054.png
   :width: 800px
.. |lab055| image:: /assets/lab1-055.png
   :width: 800px
.. |lab056| image:: /assets/lab1-056.png
   :width: 800px
.. |lab057| image:: /assets/lab1-057.png
   :width: 800px
.. |labend| image:: /assets/labend.png
   :width: 800px
