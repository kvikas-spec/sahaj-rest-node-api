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

### Active dashboard APIs

- `GET /masters/Dashboard/management`
- `GET /masters/Dashboard/oic`

### Reference dashboard APIs

- `GET /masters/DashboardReference/BindDashboardSalesDetailsForGAAdmin`
- `GET /masters/DashboardReference/BindDashboardCustomerActivityForGAAdminJSON`
- `GET /masters/DashboardReference/BindDashboardSalesDetailsForSuperAdminJson`
- `GET /masters/DashboardReference/BindDashboardCustomerActivityForSuperAdminJSON`
- `GET /masters/DashboardReference/gail-cmp`
- `GET /masters/DashboardReference/BindDashboardSalesDetailsForSuperAdminJsonForGAILCMP`
- `GET /masters/DashboardReference/BindDashboardCustomerActivityForSuperAdminJSONForGAILCMP`
- `GET /masters/DashboardReference/GetAdvanceDashboardCustomerSummariesStatus`
- `GET /masters/DashboardReference/GetAdvanceDashboardOnBoardingData`
- `GET /masters/DashboardReference/GetAdvanceDashboardSalesData`
- `GET /masters/DashboardReference/GetAdvanceSummariesRegisteredComplaintsReport`
- `GET /masters/DashboardReference/GetSuperAdminAdvanceDashboardOnBoardingData`
- `GET /masters/DashboardReference/GetSuperAdminAdvanceDashboardSalesData`
- `GET /masters/DashboardReference/GetSuperAdminAdvanceSummariesRegisteredComplaintsReport`
- `GET /masters/DashboardReference/GetSuperAdminAdvanceGAILCMPDashboardOnBoardingData`
- `GET /masters/DashboardReference/GetSuperAdminAdvanceGAILCMPDashboardSalesData`
- `GET /masters/DashboardReference/GetSuperAdminAdvanceGAILCMPSummariesRegisteredComplaintsReport`

## Notes

- The implementation follows `routes -> controller -> service -> repository`.
- Repository queries use Sequelize raw SQL with `QueryTypes.SELECT`.
- The active dashboard route exposes only `/management` and `/oic`.
- The legacy/reference dashboard calls are exposed under `/masters/DashboardReference` with their original .NET action names.
- The server-rendered MVC model data for onboarding status and complaint cards is included under `dashboardPage` and `superAdminDashboardPage`.
- Advance report endpoints return legacy DataTables shape: `sEcho`, `iTotalRecords`, `iTotalDisplayRecords`, `aaData`.
- Live DB smoke test passed against `GailGas_Meerut` and `GailGas_Common` on host `20.153.132.13`.
- The new dashboard screen metrics are included in the API response:
  - Management: summary cards, estimated vs actual billing, sale quantity, sale amount, legacy GA Admin sales/activity payloads, key activities, pricing, complaints.
  - OIC: summary cards, sale quantity, sale amount, financial-year trends, complaints.
- Query parameters supported: `company`, `gaId`, `dateRange`, `fromDate`, `toDate`.
