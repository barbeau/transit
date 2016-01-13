## General Transit Feed Specification Reference

**Revised February 2, 2015. See [Revision History](../CHANGES.md) for more details.**

This document explains the types of files that comprise a GTFS transit feed and defines the fields used in all of those files.

### Table of Contents

### Term Definitions

This section defines terms that are used throughout this document.

* **Field required** - The field column must be included in your feed, and a value must be provided for each record. Some required fields permit an empty string as a value. To enter an empty string, just omit any text between the commas for that field. Note that 0 is interpreted as "a string of value 0", and is not an empty string. Please see the field definition for details.
* **Field optional** - The field column may be omitted from your feed. If you choose to include an optional column, each record in your feed must have a value for that column. You may include an empty string as a value for records that do not have values for the column. Some optional fields permit an empty string as a value. To enter an empty string, just omit any text between the commas for that field. Note that 0 is interpreted as "a string of value 0", and is not an empty string.
* **Dataset unique** - The field contains a value that maps to a single distinct entity within the column. For example, if a route is assigned the ID **1A**, then no other route may use that route ID. However, you may assign the ID **1A** to a location because locations are a different type of entity than routes.

### Feed Files

This specification defines the following files along with their associated content:

|  Filename | Required | Defines |
|  ------ | ------ | ------ |
|  [agency.txt](#agency.txt) | **Required** | One or more transit agencies that provide the data in this feed. |
|  [stops.txt](#stops.txt) | **Required** | Individual locations where vehicles pick up or drop off passengers. |
|  [routes.txt](#routes.txt) | **Required** | Transit routes. A route is a group of trips that are displayed to riders as a single service. |
|  [trips.txt](#trips.txt)  | **Required** | Trips for each route. A trip is a sequence of two or more stops that occurs at specific time. |
|  [stop_times.txt](#stop_times.txt)  | **Required** | Times that a vehicle arrives at and departs from individual stops for each trip. |
|  [calendar.txt](#calendar.txt)  | **Required** | Dates for service IDs using a weekly schedule. Specify when service starts and ends, as well as days of the week where service is available. |
|  [calendar_dates.txt](#calendar_dates.txt)  | Optional | Exceptions for the service IDs defined in the calendar.txt file. If calendar_dates.txt includes ALL dates of service, this file may be specified instead of calendar.txt. |
|  [fare_attributes.txt](#fare_attributes.txt)  | Optional | Fare information for a transit organization's routes. |
|  [fare_rules.txt](#fare_rules.txt)  | Optional | Rules for applying fare information for a transit organization's routes. |
|  [shapes.txt](#shapes.txt)  | Optional | Rules for drawing lines on a map to represent a transit organization's routes. |
|  [frequencies.txt](#frequencies.txt)  | Optional | Headway (time between trips) for routes with variable frequency of service. |
|  [transfers.txt](#transfers.txt)  | Optional | Rules for making connections at transfer points between routes. |
|  [feed_info.txt](#feed_info.txt)  | Optional | Additional information about the feed itself, including publisher, version, and expiration information. |

### File Requirements

The following requirements apply to the format and contents of your files:

* All files in a General Transit Feed Spec (GTFS) feed must be saved as comma-delimited text.
* The first line of each file must contain field names. Each subsection of the [Field Definitions](#Field-Definitions) section corresponds to one of the files in a transit feed and lists the field names you may use in that file.
* All field names are case-sensitive.
* Field values may not contain tabs, carriage returns or new lines.
* Field values that contain quotation marks or commas must be enclosed within quotation marks. In addition, each quotation mark in the field value must be preceded with a quotation mark. This is consistent with the manner in which Microsoft Excel outputs comma-delimited (CSV) files. For more information on the CSV file format, see http://tools.ietf.org/html/rfc4180.
The following example demonstrates how a field value would appear in a comma-delimited file:
  * **Original field value:** `Contains "quotes", commas and text`
  * **Field value in CSV file:** `"Contains ""quotes"", commas and text"`
* Field values must not contain HTML tags, comments or escape sequences.
* Remove any extra spaces between fields or field names. Many parsers consider the spaces to be part of the value, which may cause errors.
* Each line must end with a CRLF or LF linebreak character.
* Files should be encoded in UTF-8 to support all Unicode characters. Files that include the Unicode byte-order mark (BOM) character are acceptable. Please see the [Unicode FAQ](http://unicode.org/faq/utf_bom.html#BOM) for more information on the BOM character and UTF-8.
* Zip the files in your feed.

## Field Definitions

### agency.txt

File: **Required**

|  Field Name | Required | Details |
|  ------ | ------ | ------ |
|  agency_id | Optional | The **agency_id** field is an ID that uniquely identifies a transit agency. A transit feed may represent data from more than one agency. The **agency_id** is dataset unique. This field is optional for transit feeds that only contain data for a single agency. |
|  agency_name | Required | The **agency_name** field contains the full name of the transit agency. Google Maps will display this name. |
|  agency_url | Required | The **agency_url** field contains the URL of the transit agency. The value must be a fully qualified URL that includes **http**:// or **https**://, and any special characters in the URL must be correctly escaped. See http://www.w3.org/Addressing/URL/4_URI_Recommentations.html for a description of how to create fully qualified URL values. |
|  agency_timezone | Required | The **agency_timezone** field contains the timezone where the transit agency is located. Timezone names never contain the space character but may contain an underscore. Please refer to http://en.wikipedia.org/wiki/List_of_tz_zones for a list of valid values. If multiple agencies are specified in the feed, each must have the same agency_timezone. |
|  agency_lang | Optional | The **agency_lang field** contains a two-letter ISO 639-1 code for the primary language used by this transit agency. The language code is case-insensitive (both en and EN are accepted). This setting defines capitalization rules and other language-specific settings for all text contained in this transit agency's feed. Please refer to http://www.loc.gov/standards/iso639-2/php/code_list.php for a list of valid values. |
|  agency_phone | Optional | The **agency_phone field** contains a single voice telephone number for the specified agency. This field is a string value that presents the telephone number as typical for the agency's service area. It can and should contain punctuation marks to group the digits of the number. Dialable text (for example, TriMet's "503-238-RIDE") is permitted, but the field must not contain any other descriptive text. |
|  agency_fare_url | Optional | The **agency_fare_url** specifies the URL of a web page that allows a rider to purchase tickets or other fare instruments for that agency online. The value must be a fully qualified URL that includes **http**:// or **https**://, and any special characters in the URL must be correctly escaped. See http://www.w3.org/Addressing/URL/4_URI_Recommentations.html for a description of how to create fully qualified URL values. |

### stops.txt

File: **Required**

|  Field Name | Required | Details |  |  |
|  ------ | ------ | ------ | ------ | ------ |
|  stop_id | **Required** | The **stop_id** field contains an ID that uniquely identifies a stop or station. Multiple routes may use the same stop. The **stop_id** is dataset unique. |  |  |
|  stop_code | Optional | The **stop_code** field contains short text or a number that uniquely identifies the stop for passengers. Stop codes are often used in phone-based transit information systems or printed on stop signage to make it easier for riders to get a stop schedule or real-time arrival information for a particular stop.  The stop_code field contains short text or a number that uniquely identifies the stop for passengers.  For internal codes, use **stop_id**. This field should be left blank for stops without a code. |  |  |
|  stop_name | **Required** | The **stop_name** field contains the name of a stop or station. Please use a name that people will understand in the local and tourist vernacular. |  |  |
|  stop_desc | Optional | The **stop_desc** field contains a description of a stop. Please provide useful, quality information. Do not simply duplicate the name of the stop. |  |  |
|  stop_lat | **Required** | The **stop_lat** field contains the latitude of a stop or station. The field value must be a valid WGS 84 latitude. |  |  |
|  stop_lon | **Required** | The **stop_lon** field contains the longitude of a stop or station. The field value must be a valid WGS 84 longitude value from -180 to 180. |  |  |
|  zone_id | Optional | The **zone_id** field defines the fare zone for a stop ID. Zone IDs are required if you want to provide fare information using [fare_rules.txt](#fare_rules.txt). If this stop ID represents a station, the zone ID is ignored. |  |  |
|  stop_url | Optional | The **stop_url** field contains the URL of a web page about a particular stop. This should be different from the agency_url and the route_url fields.  The value must be a fully qualified URL that includes **http**:// or **https**://, and any special characters in the URL must be correctly escaped. See http://www.w3.org/Addressing/URL/4_URI_Recommentations.html for a description of how to create fully qualified URL values. |  |  |
|  location_type | Optional | The **location_type** field identifies whether this stop ID represents a stop or station. If no location type is specified, or the location_type is blank, stop IDs are treated as stops. Stations may have different properties from stops when they are represented on a map or used in trip planning.  The location type field can have the following values: |  |  |
|   |  | * **0** or blank - Stop. A location where passengers board or disembark from a transit vehicle. |  |  |
|   |  | * **1** - Station. A physical structure or area that contains one or more stop. |  |  |
|  parent_station | Optional | For stops that are physically located inside stations, the **parent_station** field identifies the station associated with the stop. To use this field, stops.txt must also contain a row where this stop ID is assigned location type=1. |  |  |
|   |  | **This stop ID represents...** | **This entry's location type...** | **This entry's parent_station field contains...** |
|   |  | A stop located inside a station. | 0 or blank | The stop ID of the station where this stop is located. The stop referenced by parent_station must have location_type=1. |
|   |  | A stop located outside a station. | 0 or blank | A blank value. The parent_station field doesn't apply to this stop. |
|   |  | A station. | 1 | A blank value. Stations can't contain other stations. |
|  stop_timezone | Optional | The **stop_timezone** field contains the timezone in which this stop or station is located. Please refer to [Wikipedia List of Timezones](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) for a list of valid values. If omitted, the stop should be assumed to be located in the timezone specified by **agency_timezone** in [agency.txt](#agency.txt).   When a stop has a parent station, the stop is considered to be in the timezone specified by the parent station's **stop_timezone** value. If the parent has no stop_timezone value, the stops that belong to that station are assumed to be in the timezone specified by **agency_timezone**, even if the stops have their own **stop_timezone** values. In other words, if a given stop has a **parent_station** value, any **stop_timezone** value specified for that stop must be ignored.  Even if **stop_timezone** values are provided in stops.txt, the times in [stop_times.txt](#stop_times.txt) should continue to be specified as time since midnight in the timezone specified by **agency_timezone** in agency.txt. This ensures that the time values in a trip always increase over the course of a trip, regardless of which timezones the trip crosses. |  |  |
|  wheelchair_boarding | Optional | The **wheelchair_boarding field** identifies whether wheelchair boardings are possible from the specified stop or station. The field can have the following values: |  |  |
|   |  | * **0** (or empty) - indicates that there is no accessibility information for the stop |  |  |
|   |  | * **1** - indicates that at least some vehicles at this stop can be boarded by a rider in a wheelchair |  |  |
|   |  | * **2** - wheelchair boarding is not possible at this stop |  |  |
|   |  | When a stop is part of a larger station complex, as indicated by a stop with a **parent_station** value, the stop's **wheelchair_boarding** field has the following additional semantics: |  |  |
|   |  | * **0** (or empty) - the stop will inherit its **wheelchair_boarding** value from the parent station, if specified in the parent |  |  |
|   |  | * **1** - there exists some accessible path from outside the station to the specific stop / platform |  |  |
|   |  | * **2** - there exists no accessible path from outside the station to the specific stop / platform |  |  |