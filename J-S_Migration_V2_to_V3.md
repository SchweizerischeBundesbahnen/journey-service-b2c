# Journey-Service (J-S) migration v2 to v3
Unfortunately relating to Transmodel comes with some serious breaking model changes in v3.

## SBB API Principles
see [SBB API Principles](https://schweizerischebundesbahnen.github.io/api-principles/) enforce [Zalando API Guidelines](https://opensource.zalando.com/restful-api-guidelines/), therefore some implementations aspects of v2 are refactored in v3:
* **Enum's** are consequently modeles as plain String's
* **Boolean** won't be used as tri-state with null as valid option anymore [Zalando rule](https://opensource.zalando.com/restful-api-guidelines/#122)
* no more List<> are returned -> proper Response Object always

## Header
* Obsolete and proprietary "Log-Context" is replaced by **Request-ID** **OpenTelemetry/W3C Standard Header** and supported by Instana tracing

## Type mappings

Remark:
* Obvious (quiet similar named) mappings are not mentioned explicitely below.


| J-S v3                   | J-S v2                          | J-A                           |
| ------------------------ | ------------------------------- |-----------------------------  |
| AbstractPlace            | see J-A                         | Location                      |
| ScheduledStopPoint       | StopV2                          | OrigDestType / StopType       |
| GeometryObject (GeoJSON) | see J-A                         | CoordinatesWGS84              |
| ServiceJourneyPattern    | see J-A                         | JourneySegment                |
| PTRideLeg                | LegV2 (::type=PUBLIC_TRANSPORT) | Leg (::type=PUBLIC_TRANSPORT) |
| AccessLeg                | LegV2 (::type=FOOTPATH)         | Leg (::type=FOOTPATH)         |
| PTConnectionLeg          | LegV2 (::type=TRANSFER)         | Leg (::type=TRANSFER)         |
| TripPattern              | TripSummaryV2                   | TripSummary                   |
| Notice                   | see J-A                         | Note                          |
| PTSituationMessage       | HimMessageV2                    | Message                       |
| Line + Operator          | TransportProductV2              | ProductType                   |
| ServiceCalendar          | ServiceDaysV2                   | ServiceDays                   |
| Problem + HttpStatus     | Error + HttpStatus              | Throwable/Exception           |

## API's
| J-S v3                   | J-S v2                          | J-A                           |
| ------------------------ | ------------------------------- |-----------------------------  |
| /places                  | /locations                      | LocationAssistant             |
| /trips                   | /trips                          | TripAssistant                 |
|                          | /departures, /arrivals          | StationboardAssistant         |
|                          | /routes                         | RouteAssistant                |
|                          | /traffic                        | HimAssistant                  |
|                          | /info                           | TimetableAssistant            |
| see /trips               | /trainFormation                 | TrainFormationFacade          |


## Generated ApiClient
OpenApi 3 based, see [Swagger2 -> OpenAPI 3](https://code.sbb.ch/projects/KI_FAHRPLAN/repos/journey-service/browse/journey-service-client/SwitchingSwagger2ToOpenApi3.md)
