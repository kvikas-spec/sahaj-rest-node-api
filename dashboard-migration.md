# Dashboard API Migration

## Source

- .NET controller: `GAIL_GAS/Controllers/DashboardController.cs`
- .NET view models: `GAIL_GAS/Models/DashboardVM.cs`
- Migrated flows:
  - `DashboardController.Dashboard`
  - `DashboardController.SuperAdminDashboard`
  - `BindDashboardCustomerStatusCntForGAAdmin`
  - `BindDashboardCustomerStatusCntForSuperAdmin`
  - `BindDashboardSalesDetailsForGAAdmin`
  - `BindDashboardCustomerActivityForGAAdminJSON`
  - `BindDashboardSalesDetailsForSuperAdminJson`
  - `BindDashboardCustomerActivityForSuperAdminJSON`
  - `BindDashboardCustomerComplaintForGaAdmin`
  - `BindDashboardCustomerComplaintForSuperAdmin`
  - `SuperAdminDashboardForGAILCMP`
  - `BindDashboardSalesDetailsForSuperAdminJsonForGAILCMP`
  - `BindDashboardCustomerActivityForSuperAdminJSONForGAILCMP`
  - `GetAdvanceDashboardCustomerSummariesStatus`
  - `GetAdvanceDashboardOnBoardingData`
  - `GetAdvanceDashboardSalesData`
  - `GetAdvanceSummariesRegisteredComplaintsReport`
  - `GetSuperAdminAdvanceDashboardOnBoardingData`
  - `GetSuperAdminAdvanceDashboardSalesData`
  - `GetSuperAdminAdvanceSummariesRegisteredComplaintsReport`
  - `GetSuperAdminAdvanceGAILCMPDashboardOnBoardingData`
  - `GetSuperAdminAdvanceGAILCMPDashboardSalesData`
  - `GetSuperAdminAdvanceGAILCMPSummariesRegisteredComplaintsReport`

## Node API

- `GET /masters/Dashboard/management`
- `GET /masters/Dashboard/oic`
- `GET /masters/Dashboard/BindDashboardSalesDetailsForGAAdmin`
- `GET /masters/Dashboard/BindDashboardCustomerActivityForGAAdminJSON`
- `GET /masters/Dashboard/BindDashboardSalesDetailsForSuperAdminJson`
- `GET /masters/Dashboard/BindDashboardCustomerActivityForSuperAdminJSON`
- `GET /masters/Dashboard/gail-cmp`
- `GET /masters/Dashboard/BindDashboardSalesDetailsForSuperAdminJsonForGAILCMP`
- `GET /masters/Dashboard/BindDashboardCustomerActivityForSuperAdminJSONForGAILCMP`
- `GET /masters/Dashboard/GetAdvanceDashboardCustomerSummariesStatus`
- `GET /masters/Dashboard/GetAdvanceDashboardOnBoardingData`
- `GET /masters/Dashboard/GetAdvanceDashboardSalesData`
- `GET /masters/Dashboard/GetAdvanceSummariesRegisteredComplaintsReport`
- `GET /masters/Dashboard/GetSuperAdminAdvanceDashboardOnBoardingData`
- `GET /masters/Dashboard/GetSuperAdminAdvanceDashboardSalesData`
- `GET /masters/Dashboard/GetSuperAdminAdvanceSummariesRegisteredComplaintsReport`
- `GET /masters/Dashboard/GetSuperAdminAdvanceGAILCMPDashboardOnBoardingData`
- `GET /masters/Dashboard/GetSuperAdminAdvanceGAILCMPDashboardSalesData`
- `GET /masters/Dashboard/GetSuperAdminAdvanceGAILCMPSummariesRegisteredComplaintsReport`

## Notes

- The implementation follows `routes -> controller -> service -> repository`.
- Repository queries use Sequelize raw SQL with `QueryTypes.SELECT`.
- The four legacy AJAX calls used by `Dashboard.cshtml` and `SuperAdminDashboard.cshtml` are exposed with their original .NET action names.
- The server-rendered MVC model data for onboarding status and complaint cards is included under `dashboardPage` and `superAdminDashboardPage`.
- Advance report endpoints return legacy DataTables shape: `sEcho`, `iTotalRecords`, `iTotalDisplayRecords`, `aaData`.
- Live DB smoke test passed against `GailGas_Meerut` and `GailGas_Common` on host `20.153.132.13`.
- The new dashboard screen metrics are included in the API response:
  - Management: summary cards, estimated vs actual billing, sale quantity, sale amount, legacy GA Admin sales/activity payloads, key activities, pricing, complaints.
  - OIC: summary cards, sale quantity, sale amount, financial-year trends, complaints.
- Query parameters supported: `company`, `gaId`, `dateRange`, `fromDate`, `toDate`.
