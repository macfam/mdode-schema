# MDODE Disruption Feed Schema

This document provides a detailed breakdown of the MDODE Disruption Feed schema, used for exchanging information about verified major disruptions such as work zones, crashes, and weather events.

## JSON Schema

The complete JSON schema for the MDODE Disruption Feed can be found [here](https://github.com/macfam/mdode-schema/blob/main/schema.json).

## Table of Contents

- [Primary Structure and Properties](#mdode-disruption-feed-schema)
- [Properties Object Details](#properties-object-details)
- [Geometry Details](#geometry-details)
- [Properties Details](#properties-details)
- [Severity Details](#severity-details)
- [Source Agency Details](#source-agency-details)
- [Verification Details](#verification-details)
- [Data Quality Details](#data-quality-details)



## MDODE Disruption Feed Schema

This table outlines the main structure and properties of the MDODE Disruption Feed schema.

| Field         | Type   | Description                                     | Required |
| :------------ | :----- | :---------------------------------------------- | :------- |
| `$schema`     | string | JSON Schema version. `http://json-schema.org/draft-07/schema#` | No       |
| `title`       | string | Schema title: "MDODE Disruption Feed"           | No       |
| `description` | string | Describes the purpose of the schema.            | No       |
| `type`        | string | The root element is an "object".                | No       |
| `required`    | array  | Specifies the required fields: `id`, `type`, `geometry`, `properties`. | Yes      |
| `properties`  | object | Contains the definitions for each property within the MDODE Disruption Feed. | No       |

## Properties Object Details

This section describes the properties defined within the main `properties` object.

| Field        | Type   | Description                                      | Required | Details                             |
| :----------- | :----- | :----------------------------------------------- | :------- | :---------------------------------- |
| `id`         | string | Unique identifier for the disruption.            | Yes      | Example: UUID                       |
| `type`       | string | GeoJSON Feature type.                            | Yes      | Must be "Feature".                   |
| `geometry`   | object | GeoJSON geometry object representing the disruption's location. | Yes      | See Geometry Details table below. |
| `properties` | object | Contains details about the disruption.           | Yes      | See Properties Details table below. |

## Geometry Details

Details the structure of the `geometry` object.

| Field         | Type   | Description                          | Required | Enum/Details                                      |
| :------------ | :----- | :----------------------------------- | :------- | :-------------------------------------------------- |
| `type`        | string | GeoJSON geometry type.             | Yes      | `"LineString"` (for road closures) or `"Point"` (for localized events). |
| `coordinates` | array  | WGS84 coordinate array.            | Yes      | Array of numbers.  Example: `[[-83.123, 40.456], [-83.124, 40.457]]` for LineString.<br>Just `[-83.123, 40.456]` for a Point |

## Properties Details

Details the structure of the nested `properties` object.

| Field          | Type   | Description                              | Required |
| :------------- | :----- | :--------------------------------------- | :------- |
| `description`  | string | Human-readable summary of the disruption. | Yes      |
| `cause`        | string | Type of disruption.                      | Yes      |
| `severity`     | object | Impact details.                          | Yes      |
| `source_agency`| object | Agency responsible for verification.   | Yes      |
| `last_updated` | string | ISO 8601 timestamp of last update.      | Yes      |
| `verification` | object | How the disruption was validated.        | No       |
| `data_quality` | object | Embedded quality metrics.                | No       |

## Severity Details

| Field             | Type    | Description                               | Required | Details |
| :---------------- | :------ | :---------------------------------------- | :------- | :------- |
| `lanes_closed`    | integer | Number of lanes affected.               | No       | Minimum value: 0 |
| `directions_closed` | array   | Directional closures (if any).          | No       | Array of strings. Example: `["northbound", "southbound"]`.<br>Enum: `northbound`, `southbound`, `eastbound`, `westbound` |

## Source Agency Details

| Field   | Type   | Description                      | Required |
| :------ | :----- | :------------------------------- | :------- |
| `name`  | string | Name of the agency.              | Yes      |
| `contact` | object | Contact information for agency.  | Yes      |

### Source Agency Contact Details

| Field   | Type   | Description                       | Required |
| :------ | :----- | :-------------------------------- | :------- |
| `email` | string | Email address of contact person.  | No       |
| `phone` | string | Phone number of contact person. | No       |

## Verification Details

| Field     | Type   | Description                           | Required | Enum/Details                                |
| :-------- | :----- | :------------------------------------ | :------- | :------------------------------------------ |
| `method`    | string | How the disruption was validated.     | Yes      | `"automated_sensor"`, `"manual_patrol"`, or `"third_party"`. |
| `timestamp` | string | When verification occurred (ISO 8601 timestamp). | Yes      | Format: date-time                           |

## Data Quality Details

| Field            | Type    | Description                               | Required | Details                   |
| :--------------- | :------ | :---------------------------------------- | :------- | :------------------------ |
| `confidence_score` | number  | Provider’s confidence in accuracy.      | No       | Minimum: 0, Maximum: 100  |
| `validation_status`| string | TN-ITS-style tier.                        | No       | Enum: `"bronze"`, `"silver"`, `"gold"` |
