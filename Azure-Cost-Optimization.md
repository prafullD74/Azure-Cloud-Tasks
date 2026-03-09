# Cost 
- Monitor and analyze your Azure bill with Microsoft Cost Management. Set budgets and allocate spending to your teams and projects. Estimate the costs for your next Azure projects using the Azure pricing calculator. Successfully build your cloud business case with key financial and technical guidance from Azure.
- Follow your Azure Advisor best practice recommendations for cost savings. Review your workload architecture for cost optimization using the Microsoft Azure Well-Architected Review assessment and the Microsoft Azure Well-Architected Framework design documentation. Save with Azure offers and licensing terms such as the Azure Hybrid Benefit, paying in advance for predictable workloads with reservations, Azure Spot Virtual Machines, Azure savings plan for compute, and Azure dev/test pricing.

## [Microsoft Cost Management](https://learn.microsoft.com/en-us/azure/cost-management-billing/costs/overview-cost-management)
1. Microsoft Cost Management is a suite of FinOps tools that help organizations analyze, monitor, and optimize their Microsoft Cloud costs.
2. [Cost Management](https://www.youtube.com/c/AzureCostManagement) works with Azure Advisor to provide cost optimization recommendations. [Optimizing cloud investments in Cost Management](https://www.youtube.com/watch?v=cSNPoAb-TNc)
3. Azure Advisor helps you optimize and improve efficiency by identifying idle and underutilized resources. [Tutorial: Optimize costs from recommendations](https://learn.microsoft.com/en-us/azure/cost-management-billing/costs/tutorial-acm-opt-recommendations)

## [Microsoft Cost Management](https://learn.microsoft.com/en-us/azure/cost-management-billing/costs/overview-cost-management#optimize-costs)
1. [Cost Management](https://portal.azure.com/#view/Microsoft_Azure_CostManagement/Menu) is a set of FinOps tools that enable you to analyze, manage, and optimize your costs.
2. [Billing](https://portal.azure.com/#view/Microsoft_Azure_GTM/ModernBillingMenuBlade) provides all the tools you need to manage your billing account and pay invoices.
3. [Estimate your cloud costs](https://learn.microsoft.com/en-us/azure/cost-management-billing/costs/overview-cost-management#estimate-your-cloud-costs)
   1. [Azure Migrate](https://azure.microsoft.com/products/azure-migrate/) is a free tool that helps you analyze your on-premises workloads and plan your cloud migration.
   2. The [Azure pricing calculator](https://azure.microsoft.com/pricing/calculator/) is a free cost management tool that allows users to understand and estimate costs of Azure Services and products. 
4. [Report on and analyze costs](https://learn.microsoft.com/en-us/azure/cost-management-billing/costs/overview-cost-management#report-on-and-analyze-costs)
   1. [Cost Analysis](https://learn.microsoft.com/en-us/azure/cost-management-billing/costs/quick-acm-cost-analysis) is a tool for ad-hoc cost exploration. Get quick answers with lightweight insights and analytics. 
   2. [Exports and the Cost Details API](https://learn.microsoft.com/en-us/azure/cost-management-billing/automate/usage-details-best-practices) enable you to integrate cost details into external systems or business processes.
5. [Monitor costs with alerts](https://learn.microsoft.com/en-us/azure/cost-management-billing/costs/overview-cost-management#monitor-costs-with-alerts)
   1. [Budget alerts](https://learn.microsoft.com/en-us/azure/cost-management-billing/costs/tutorial-acm-create-budgets) notify recipients when cost exceeds a predefined cost or forecast amount.
   2. [Anomaly alerts](https://learn.microsoft.com/en-us/azure/cost-management-billing/understand/analyze-unexpected-charges) notify recipients when an unexpected change in daily usage has been detected.
   3. [Scheduled alerts](https://learn.microsoft.com/en-us/azure/cost-management-billing/costs/save-share-views#subscribe-to-scheduled-alerts) notify recipients about the latest costs on a daily, weekly, or monthly schedule based on a saved cost view in Cost Analysis.
6. [Optimize costs](https://learn.microsoft.com/en-us/azure/cost-management-billing/costs/overview-cost-management#optimize-costs)
   1. [Azure Advisor cost recommendations](https://learn.microsoft.com/en-us/azure/cost-management-billing/costs/tutorial-acm-opt-recommendations) should be your first stop when interested in optimizing existing resources. 
      - Cost Management works with Azure Advisor to provide cost optimization recommendations. Azure Advisor helps you optimize and improve efficiency by identifying idle and underutilized resources. [Tutorial: Optimize costs from recommendations](https://learn.microsoft.com/en-us/azure/cost-management-billing/costs/tutorial-acm-opt-recommendations)
      - Azure Advisor monitors your virtual machine usage for seven days and then identifies underutilized virtual machines. Virtual machines whose CPU utilization is five percent or less and network usage is seven MB or less for four or more days are considered low-utilization virtual machines.
      - The 5% or less CPU utilization setting is the default, but you can adjust the settings. For adjusting the setting >> [Configure the average CPU utilization rule or the low usage virtual machine recommendation](https://learn.microsoft.com/en-us/azure/advisor/advisor-get-started#configure-recommendations)
   2. [Azure savings plans ](https://learn.microsoft.com/en-us/azure/cost-management-billing/savings-plan/)
   3. [Azure reservations](https://azure.microsoft.com/reservations/)
   4. [Azure Hybrid Benefit]([Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-benefit/))

### 8 ways to optimize costs today
1. [Shut down unused resources](https://go.microsoft.com/fwlink/?linkid=2237711&clcid=0x4009)
   - Identify idle virtual machines (VMs), ExpressRoute circuits, and other resources with Azure Advisor. Get recommendations on which resources to shut down, and see how much you would save.
2. [Right-size underused resources](https://go.microsoft.com/fwlink/?linkid=2237487&clcid=0x4009)
   - Find underutilized resources with Azure Advisor—and get recommendations on how to reduce your spend by reconfiguring or consolidating them.
3. [Add an Azure savings plan for compute for dynamic workloads](https://azure.microsoft.com/en-in/pricing/offers/savings-plan-compute/)
   - Save up to 65% off pay-as-you-go pricing when you commit to spend a fixed hourly amount on compute services for one or three years.
4. [Reserve instances for consistent workloads](https://azure.microsoft.com/en-in/reservations/)
   - Get a discount of up to 72% over pay-as-you-go pricing on Azure services when you prepay for a one- or three-year term with reservation pricing.
5. [Take advantage of the Azure Hybrid Benefit](https://azure.microsoft.com/en-in/pricing/hybrid-benefit/)
   - Save when you migrate your on-premises workloads to Azure. Windows Server customers can save up to 36%, and SQL Server customers can save up to 28% compared to the leading cloud provider.
6. [Configure autoscaling](https://go.microsoft.com/fwlink/?linkid=2237613&clcid=0x4009)
   - Save by dynamically allocating and deallocating resources to match your performance needs.
7. [Choose the right Azure compute service](https://go.microsoft.com/fwlink/?linkid=2237614&clcid=0x4009)
   - Host your code using one of the many ways Azure offers. Operate more cost efficiently by selecting the right compute service for your application.
8. [Set up budgets and allocate costs to teams and projects](https://go.microsoft.com/fwlink/?linkid=2237292&clcid=0x4009)
   - Create and manage budgets for the Azure services you use or subscribe to—and monitor your organization’s cloud spending—with Microsoft Cost Management.

### [Defining roles and responsibilities for cloud cost optimization](https://azure.microsoft.com/en-us/blog/defining-roles-and-responsibilities-for-cloud-cost-optimization/)
