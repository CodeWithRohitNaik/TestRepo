# SharePoint list schema mapping

Source: `Fields.xml` (SharePoint list fields export).

This mapping focuses on the 33 visible business fields. SharePoint's built-in fields and hidden fields are omitted unless needed for migration. SQL types below are recommendations; confirm number precision and actual data before applying a migration.

## Business review checklist

Please ask the business owner to complete this checklist before finalizing the SQL schema and MVC screens:

- Mark **Include?** for fields the new app should use. Leave it unchecked if the field is not needed.
- Fill in **Required?** based on the new app's business rules; SharePoint required settings are only a reference.
- Fill in **Editable?** to confirm whether users can change the value or it is calculated/read-only.
- Add any business-specific validation, workflow, or migration notes.

| Field | Include? | Required? | Editable? | Business notes |
|---|---|---|---|---|
| Engine SN | [ ] |  |  |  |
| Margin | [ ] |  |  |  |
| General Remarks | [ ] |  |  |  |
| Test Cell / Last Flight | [ ] |  |  |  |
| 720 Due | [ ] |  |  |  |
| CANN Task Status | [ ] |  |  |  |
| Check Preservation | [ ] |  |  |  |
| Classification | [ ] |  |  |  |
| Current Location | [ ] |  |  |  |
| Est. Departure Date | [ ] |  |  |  |
| Engine Ship Category | [ ] |  |  |  |
| Engine Status | [ ] |  |  |  |
| Est Arrival Date | [ ] |  |  |  |
| Indefinite Preservation Date | [ ] |  |  |  |
| In Transit | [ ] |  |  |  |
| Last 15 Day Humidifier Inspection | [ ] |  |  |  |
| Last Preservation | [ ] |  |  |  |
| Next 15 Day Humidifier Inspection Due | [ ] |  |  |  |
| Next Preservation Due | [ ] |  |  |  |
| On Dock Date | [ ] |  |  |  |
| Preservation Type | [ ] |  |  |  |
| Scheduled Preservation | [ ] |  |  |  |
| Ship Destination | [ ] |  |  |  |
| Ship Origination | [ ] |  |  |  |
| Speedline | [ ] |  |  |  |
| 60 Days for Alt I 180 | [ ] |  |  |  |
| Spare Engines | [ ] |  |  |  |
| Training Engine | [ ] |  |  |  |
| Unserv Report Group | [ ] |  |  |  |
| Assigned to Ship | [ ] |  |  |  |
| Assigned | [ ] |  |  |  |
| In Grief Notes | [ ] |  |  |  |

## Business fields

| SharePoint display name | SharePoint internal name | SharePoint type | Suggested SQL Server column | Notes |
|---|---|---|---|---|
| Engine SN | `Title` | Text (required) | `EngineSN nvarchar(255) NOT NULL` | SharePoint's required `Title` field is displayed as Engine SN. |
| Margin | `Margin` | Number | `Margin decimal(18,4) NULL` | Confirm precision and scale against source values. |
| General Remarks | `General_x0020_Remarks` | Note | `GeneralRemarks nvarchar(max) NULL` | Multiple lines, rich text enabled. |
| Test Cell / Last Flight | `Test_x0020_Cell_x0020__x002f__x0` | DateTime | `TestCellLastFlight date NULL` | Date-only field. |
| 720 Due | `_x0037_20_x0020_Due` | Calculated | `Due720 date NULL` | Formula shown under [Calculated fields](#calculated-fields). |
| CANN Task Status | `CANN_x0020_In_x0020_Progress_x00` | Boolean | `CANNTaskStatus bit NULL` |  |
| Check Preservation | `Check_x0020_Preservation` | Choice | `CheckPreservation nvarchar(50) NULL` | Values listed under [Choice fields](#choice-fields). |
| Classification | `Classification` | Choice | `Classification nvarchar(30) NULL` | Values listed under [Choice fields](#choice-fields). |
| Current Location | `Current_x0020_Location` | Choice | `CurrentLocation nvarchar(255) NULL` | Values listed under [Choice fields](#choice-fields). |
| Est. Departure Date | `Est_x002e__x0020_Departure_x0020` | DateTime | `EstimatedDepartureDate date NULL` | Date-only field. |
| Engine Ship Category | `Engine_x0020_Ship_x0020_Category` | Choice | `EngineShipCategory nvarchar(30) NULL` | Values listed under [Choice fields](#choice-fields). |
| Engine Status | `Engine_x0020_Status` | Calculated | `EngineStatus nvarchar(50) NULL` | Formula shown under [Calculated fields](#calculated-fields). |
| Est Arrival Date | `Est_x0020_Arrival_x0020_Date` | DateTime | `EstimatedArrivalDate date NULL` | Date-only field. |
| Indefinite Preservation Date | `Indefinite_x0020_Preservation_x0` | DateTime | `IndefinitePreservationDate date NULL` | Date-only field. |
| In Transit | `In_x0020_Transit` | Boolean | `InTransit bit NULL` |  |
| Last 15 Day Humidifier Inspection | `Last_x0020_15_x0020_Day_x0020_Hu` | DateTime | `LastHumidifierInspectionDate date NULL` | Date-only field. |
| Last Preservation | `Last_x0020_Preservation` | DateTime | `LastPreservation date NULL` | Date-only field. |
| Next 15 Day Humidifier Inspection Due | `Next_x0020_15_x0020_Day_x0020_Hu` | Calculated | `NextHumidifierInspectionDue date NULL` | Formula shown under [Calculated fields](#calculated-fields). |
| Next Preservation Due | `Next_x0020_Preservation_x0020_Du` | Calculated | `NextPreservationDue date NULL` | Formula shown under [Calculated fields](#calculated-fields). |
| On Dock Date | `On_x0020_Dock_x0020_Date` | DateTime | `OnDockDate date NULL` | Date-only field. |
| Preservation Type | `Preservation_x0020_Type` | Choice | `PreservationType nvarchar(100) NULL` | Values listed under [Choice fields](#choice-fields). |
| Scheduled Preservation | `Scheduled_x0020_Preservation` | DateTime | `ScheduledPreservation date NULL` | Date-only field. |
| Ship Destination | `Ship_x0020_Destination` | Choice | `ShipDestination nvarchar(255) NULL` | Values listed under [Choice fields](#choice-fields). |
| Ship Origination | `Ship_x0020_Origination` | Choice | `ShipOrigination nvarchar(255) NULL` | Values listed under [Choice fields](#choice-fields). |
| Speedline | `Speedline` | Boolean | `Speedline bit NULL` |  |
| 60 Days for Alt I 180 | `_x0036_0_x0020_Days_x0020_for_x0` | Calculated | `AltI180DueDate date NULL` | Formula shown under [Calculated fields](#calculated-fields). |
| Spare Engines | `Spare_x0020_Engines` | Calculated | `SpareEngines nvarchar(50) NULL` | Formula shown under [Calculated fields](#calculated-fields). |
| Training Engine | `Training_x0020_Engine` | Boolean | `TrainingEngine bit NULL` |  |
| Unserv Report Group | `Unserv_x0020_Report_x0020_Group` | Calculated | `UnserviceableReportGroup nvarchar(100) NULL` | Formula shown under [Calculated fields](#calculated-fields). |
| Assigned to Ship | `Assigned_x0020_to_x0020_Ship0` | Text | `AssignedToShip nvarchar(255) NULL` |  |
| Assigned | `Assigned` | Text | `Assigned nvarchar(255) NULL` |  |
| In Grief Notes | `In_x0020_Grief_x0020_Notes` | Note | `InGriefNotes nvarchar(max) NULL` | Multiple lines, rich text enabled. |

## Choice fields

The schema defines these as single-value SharePoint Choice fields, not SharePoint Lookup fields. For a simple schema, store the selected label in an `nvarchar` column. If using SQL reference tables for MVC dropdowns, create a separate option set per field and migrate each source label to its corresponding SQL key. Preserve the exact labels, including punctuation and spelling.

| Field | Choice values |
|---|---|
| Check Preservation | `Check Preservation`; `Check 720`; `Awaiting Records` |
| Classification | `SPS`; `WRE`; `UNSERV`; `INSTALLED`; `IN-REPAIR`; `IN GRIEF` |
| Current Location | `Ali Al Salem, Kuwait`; `Altus AFB, OK`; `Al-Udeid, Qatar (En-route)`; `Andersen AFB Guam`; `Boeing Long Beach, CA (Production/Depot)`; `BSS- San Antonio, TX (Aircraft Depot)`; `Charlotte ANG, NC`; `Columbus, GA (Depot)`; `CTS Engines, FL (Depot)`; `Delta TechOps, GA (Depot)`; `Dover AFB, DE`; `Davis-Monthan, AZ`; `Edwards AFB, CA (Testing)`; `FMS-Al Udeid, Qatar`; `FMS-CAF Trenton, Canada`; `FMS-Hindan, India`; `FMS-RAAF Amberley, Australia`; `FMS-RAF Brize Norton, United Kingdom`; `FMS-SAC/NAM Papa, Hungary`; `FMS-United Arab Emirates`; `FMS-Al Mubarak, Kuwait`; `Fort A.P. Hill AAF`; `Gray Field (AAF)`; `GXO-Logistics, SC`; `Jackson ANG, MS`; `Joint-Base Lindsey Graham, SC`; `Joint-Base Lindsey Graham, SC (RAMS)`; `Joint-Base Elmendorf-Richardson, AK`; `Joint-Base Lewis-McChord, WA`; `Joint-Base McGuire-Dix-Lakehurst, NJ`; `Joint-Base Pearl Harbor-Hickam, HI`; `Kadena AB, Japan`; `Khartoum, Sudan`; `March ARB, CA`; `Martinsburg ANG, WV`; `Memphis ANG, TN`; `Pittsburgh ARB, PA`; `Prince Sultan AB, Saudi Arabia`; `Ramstein, Germany (En-Route)`; `Rota AB, Spain (En-Route)`; `San Francisco, CA (Depot)`; `Sigonella, Italy (En-Route)`; `Stewart ANG, NY`; `Tinker AFB, OK (Depot)`; `Travis AFB, CA`; `Warner-Robins ALC, GA (Depot)`; `Wright-Patterson ARB, OH`; `Warner-Robins ALC, GA (Aircraft Depot)`; `Yokota AB, Japan`; `Goose Bay AB, Canada`; `Diego Garcia` |
| Engine Ship Category | `Induction`; `Serviceable`; `Unserviceable`; `Acceptance` |
| Preservation Type | `(1) SIB`; `(2) Not Preservable`; `(3) Preservable`; `(4) Preservable, Pending MX Action` |
| Ship Destination | `Ali Al Salem, Kuwait`; `Altus AFB, OK`; `Al-Udeid, Qatar (En-route)`; `Andersen AFB Guam`; `Boeing Long Beach, CA (Production/Depot)`; `BSS- San Antonio, TX (Aircraft Depot)`; `Charlotte ANG, NC`; `Columbus, GA (Depot)`; `CTS Engines, FL (Depot)`; `Delta TechOps, GA (Depot)`; `Dover AFB, DE`; `Davis-Monthan, AZ`; `Edwards AFB, CA (Testing)`; `FMS-Al Udeid, Qatar`; `FMS-CAF Trenton, Canada`; `FMS-Hindan, India`; `FMS-RAAF Amberley, Australia`; `FMS-RAF Brize Norton, United Kingdom`; `FMS-SAC/NAM Papa, Hungary`; `FMS-United Arab Emirates`; `FMS-Al Mubarak, Kuwait`; `Fort A.P. Hill AAF`; `Gray Field (AAF)`; `GXO-Logistics, SC`; `Jackson ANG, MS`; `Joint-Base Lindsey Graham, SC`; `Joint-Base Lindsey Graham, SC (RAMS)`; `Joint-Base Elmendorf-Richardson, AK`; `Joint-Base Lewis-McChord, WA`; `Joint-Base McGuire-Dix-Lakehurst, NJ`; `Joint-Base Pearl Harbor-Hickam, HI`; `Kadena AB, Japan`; `Khartoum, Sudan`; `March ARB, CA`; `Martinsburg ANG, WV`; `Memphis ANG, TN`; `Pittsburgh ARB, PA`; `Prince Sultan AB, Saudi Arabia`; `Ramstein, Germany (En-Route)`; `Rota AB, Spain (En-Route)`; `San Francisco, CA (Depot)`; `Sigonella, Italy (En-Route)`; `Stewart ANG, NY`; `Tinker AFB, OK`; `Travis AFB, CA`; `Warner-Robins ALC, GA (Depot)`; `Wright-Patterson ARB, OH`; `Warner-Robins ALC, GA (Aircraft Depot)`; `Yokota AB, Japan` |
| Ship Origination | Same as Ship Destination, plus `In-Transit` |

The Current Location, Ship Destination, and Ship Origination choice sets are not identical. In particular, Ship Destination has `Tinker AFB, OK` while Current Location has `Tinker AFB, OK (Depot)`, and Ship Origination includes `In-Transit`.

## Calculated fields

These are the formulas present in the SharePoint schema. When importing data, choose whether to migrate SharePoint's computed result or recompute it in the application/database. If recomputing, validate SQL/.NET behavior against SharePoint for null and text edge cases.

| Field | SharePoint formula |
|---|---|
| 720 Due | `=IF([Test Cell / Last Flight]="","",[Test Cell / Last Flight]+720)` |
| Engine Status | `=IF(Classification="SPS","SPS",IF(Classification="WRE","SPS",IF(Classification="","",IF(Classification="INSTALLED","INSTALLED",IF(Classification="UNSERV","Unserviceable",IF(Classification="IN-REPAIR","Unserviceable",IF(Classification="In Grief","Unserviceable","")))))))` |
| Next 15 Day Humidifier Inspection Due | `=IF([Last 15 Day Humidifier Inspection]="","",[Last 15 Day Humidifier Inspection]+15)` |
| Next Preservation Due | `=IF([Last Preservation]="","",[Last Preservation]+180)` |
| 60 Days for Alt I 180 | `=IF([Test Cell / Last Flight]="","",IF([Last Preservation]<>"","",IF(Classification="WRE","",IF(Classification="SPS","",[Test Cell / Last Flight]+60))))` |
| Spare Engines | `=IF(Classification="","",IF(Classification<>"INSTALLED","Spare",""))` |
| Unserv Report Group | `=IF([Current Location]=OR("Columbus, GA (Depot)","Tinker AFB, OK (Depot)","Warner-Robins ALC, GA (Depot)"),"DEPOT",IF(LEFT([Current Location],4)="FMS-","FMS",IF([Current Location]="GXO Logistics, SC",[Current Location],"USAF")))` |

## Migration notes

- Consider adding `SharePointId int NOT NULL` (or nullable during initial load) to retain each item's original SharePoint `ID` and support reconciliation.
- Preserve the source internal names in the import mapping even if the SQL columns use clearer application names.
- For SQL choice reference tables, include at least a key, the exact choice label, and optionally a sort order. Keep a distinct option set per field unless you intentionally normalize shared choices and enforce allowed values separately.
- Validate the proposed SQL lengths against actual list data and any future values users may add.
- The export contains system and hidden columns (for example Created, Modified, Author, Editor, and ID). Decide separately whether to migrate audit metadata and user identities; user fields may require their own identity mapping.
