# Artist-Related Endpoints in the Spotify Web API 

This guide provides you with a brief overview of the artist-related API endpoints.

## Overview

The following table provides you with an overview of the Spotify Web API endpoints related to artists.

| **Category**   | **Endpoint**   | **Method**  | **Description**  |
|--------------|-----------------|----------------|----------------|
| [**Get Artist**](#get-artist) | `artists/{id}` | `GET` | Retrieve the catalog information for an artist by their Spotify ID. |
| [**Get Several Artists**](#get-several-artists) | `/artists` | `GET` | Retrieve the catalog information for multiple artists by their Spotify IDs. |
| [**Get Artist's Albums**](#get-artists-albums) | `/artists/{id}/albums`  | `GET` | Get the catalog information of an artist's albums. |
| [**Get Artist's Top Tracks**](#get-artists-top-tracks) | `/artists/{id}/top-tracks` | `GET` | Get the catalog information of an artist's top tracks by country. |
| [**Get Artist's Related Artists**](#get-artists-related-artists) | `/artists/{id}/related-artists` | `GET` | Retrieve a list of artists that are similar to a specific artist based on Spotify users' listening history. |


## Get Artist

Retrieve the catalog information for an artist by their Spotify ID.

**Endpoint:** `artists/{id}`

### Request

**HTTP Method:** `GET`

| **Parameter**   | **Necessity**   | **Type**  | **Description**  | **Example values**  |
|--------------|-----------------|----------------|----------------|----------------|
| `id` | Required | String | The Spotify ID of the artist. | `4k1ELeJKT1ISyDv8JivPpB`


### Response – Success

When the response has been successfully completed, a `200 OK` response is returned along with the information listed in the following table.

| **Parameter**   |  **Type**  | **Description**  | **Additional data included** |
|--------------|-----------------|----------------|----------------|
| `external_urls` | Object |  External URL of the artist. | `spotify`: Spotify URL of the object. |
| `followers` | String | Information about the artist's followers. | `total`: The total number of followers. |
| `genres` |  Array of strings | The list of genres associated with the artist. For example: `["Punkˇ, ˇHardcore"]` | None. |
| `href` | String | A link to the endpoint that provides full details on the artist. | None. |
| `images` | Array of ImageObjects | Images of the artist. | `url`: The URL of the image.<br/> `height`: The height of the image.<br/>`width`: The width of the image. |
| `name` | String | The name of the artist. | None. |
| `popularity` | Integer | The popularity of the artist on a scale between *0* (the least popular) and *100* (the most popular). | None. |
| `type` | String | The object type; always `artist` for this response type. | None. |
| `uri` | String |  The Spotify URI of the artist. | None. |


### Response – Error

When the response cannot be successfully completed, one of the following error codes is returned.

| **Response**   | **Description**   | **Solution**  | 
|--------------|-----------------|-----------------|
| `401 Unauthorized` | Invalid or expired token. | Re-authenticate. |
| `403 Forbidden` | Request refused by the server. | Do not repeat the request. |
| `429 Too Many Requests` | Too many requests from the user's IP. | Repeat the request later. |


## Get Several Artists

Retrieve the catalog information for multiple artists by their Spotify IDs.

**Endpoint:** `/artists`


### Request

**HTTP Method:** `GET`

| **Parameter**   | **Necessity**   | **Type**  | **Description**  | **Example values**  |
|--------------|-----------------|----------------|----------------|----------------|
| `ids` | **Required** | String | A comma-separated list of the Spotify IDs of the artists. One request can contain maximum 50 IDs.| `5LfGQac0EIXyAN8aUwmNAQ, 6i0KVTOvm96T55mbp742ks` |


### Response – Success

When the response has been successfully completed, a `200 OK` response is returned along with the information listed in the following table.

| **Parameter**   |  **Type**  | **Description**  | **Additional data included** |
|--------------|-----------------|----------------|----------------|
| `artists` | Array of ArtistObjects | A list of the artist. | `external_urls`: External URL of the artist. </br>`followers`: Information about the artist's followers. </br>`genres`: The list of genres associated with the artist. For example: `["Punk", "Hardcore"]` </br>`href`: A link to the endpoint that provides full details on the artist. </br>`id`: The Spotify ID of the artist.</br>`images`: Images of the artist. </br>`name`: The name of the artist. </br>`popularity`: The popularity of the artist on a scale between *0* (the least popular) and *100* (the most popular). </br>`type`: The object type; always </br>`uri`: The Spotify URI of the artist.</br> |


### Response – Error

When the response cannot be successfully completed, one of the following error codes is returned.

| **Response**   | **Description**   | **Solution**  | 
|--------------|-----------------|-----------------|
| `401 Unauthorized` | Invalid or expired token. | Re-authenticate. |
| `403 Forbidden` | Request refused by the server. | Do not repeat the request. |
| `429 Too Many Requests` | Too many requests from the user's IP. | Repeat the request later. |


## Get Artist's Albums

Get the catalog information of an artist's albums.

**Endpoint:** `/artists/{id}/albums`


### Request

**HTTP Method:** `GET`

| **Parameter**   | **Necessity**   | **Type**  | **Description**  | **Example values**  |
|--------------|-----------------|----------------|----------------|----------------|
| `id` | **Required** | String | The Spotify ID of the artist. | `4k1ELeJKT1ISyDv8JivPpB` |
| `include_groups` | Optional | String |  A comma-separated list of the filters. Valid values: `album`, `single`, `appears_on`, `compilation`  | `album, compilation` |
| `market` | Optional | String | Country code. If specified, only the content available in that country is returned. | `FI` |
| `limit` | Optional | Integer | The maximum number of items returned. Any number can be specified between *0* and *50*. The default value is *20*. | `22` |
| `offset` | Optional | Integer | The index of the first item to return. The default value is *0* which means the first item. | `3` |


### Response – Success

When the response has been successfully completed, a `200 OK` response is returned along with the information listed in the following table.

| **Parameter**   |  **Type**  | **Description**  | **Additional data included** |
|--------------|-----------------|----------------|----------------|
| `href` | String |   A link to the endpoint that provides full details on the artist. | None. |
| `limit` | Integer | The maximum number of items in the response. | None. |
| `next` |  String | The URL to the next page of results, if applicable. | None. |
| `offset` | Integer | The offset of the items returned. | None. |
| `previous` | String | The URL to the previous page of results, if applicable. | None. |
| `total` | Integer | The total  number of items available to return. | None. |
| `items` | Array of SimplifiedAlbumObject | Additional data on the albums in the response. | `album_type`: The type of the album, such as `album`, `single`, or `compilation` </br>`total_tracks`: The number of tracks on the album. </br>`available_markets`: The markets in which at least one trak from the album is available. </br>`external_urls`: External URL for the album. </br>`href`: A link to the endpoint that provides full details on the album. </br>`id`: The Spotify ID of the album. </br>`image`: The cover art of the album. </br>`name`: The name of the album. </br>`release_date`: The date the album was released. </br>`release_date_precision`: The accuracy of the release date, such as `year`, `month`, or `day`. </br>`restrictions`: Included only if content restrictions apply. </br>`type`: The object type; always `album` for this response type. </br>`uri`: The Spotify URI of the artist.</br>`artists`: The artists who recorded the album.</br>`album_group`: Additional information on the album, such as `album`, `single`, `appears_on`, or `compilation` | 


### Response – Error

When the response cannot be successfully completed, one of the following error codes is returned.

| **Response**   | **Description**   | **Solution**  | 
|--------------|-----------------|-----------------|
| `401 Unauthorized` | Invalid or expired token. | Re-authenticate. |
| `403 Forbidden` | Request refused by the server. | Do not repeat the request. |
| `429 Too Many Requests` | Too many requests from the user's IP. | Repeat the request later. |


## Get Artist's Top Tracks

Get the catalog information of an artist's top tracks by country.

**Endpoint:** `/artists/{id}/top-tracks`


### Request

**HTTP Method:** `GET`

| **Parameter**   | **Necessity**   | **Type**  | **Description**  | **Example values**  |
|--------------|-----------------|----------------|----------------|----------------|
| `id` | Required | String | The Spotify ID of the artist. | `4k1ELeJKT1ISyDv8JivPpB` |
| `market` | Optional | String | Country code. If specified, only the content available in that country is returned. | `FI` |


### Response – Success

When the response has been successfully completed, a `200 OK` response is returned along with the information listed in the following table.

| **Parameter**   |  **Type**  | **Description**  | **Additional data included** |
|--------------|-----------------|----------------|----------------|
| `tracks` | Array of TrackObjects | A list of tracks. | `album`: The album that contains the track.</br>`artists`: The artists who perform the track.</br>`available_markets`: The list of countries where the track is available.</br>`disc_number`: The number of discs of the album that contains the track.</br>`duration_ms`: The length of the track in milliseconds.</br>`explicit`: Whether the track is explicit.</br>`external_ids`: External URL of the track. </br>`external_urls`: External URL of the track. </br>`href`: A link to the endpoint that provides full details on the track. </br>`id`: The Spotify ID of the track.</br>`is_playable`: It is present if the track is available in the given market. </br>`linked_from`: Information about the originally requested track in case it is not playable in the given market and is replaced with another track. </br>`restrictions`: Included only if content restrictions apply. </br>`name`: The name of the track. </br>`popularity` The popularity of the track on a scale between *0* (the least popular) and *100* (the most popular). </br>`preview_url`: A link to a 30-second preview of the track if available. </br>`track_number`: The ordinal number of the track on the album. </br>`type`: The object type; always `track` for this response type. </br>`uri`: The Spotify URI of the track.</br>`is_local`: External URL of the track. |


### Response – Error

When the response cannot be successfully completed, one of the following error codes is returned.

| **Response**   | **Description**   | **Solution**  | 
|--------------|-----------------|-----------------|
| `401 Unauthorized` | Invalid or expired token. | Re-authenticate. |
| `403 Forbidden` | Request refused by the server. | Do not repeat the request. |
| `429 Too Many Requests` | Too many requests from the user's IP. | Repeat the request later. |


## Get Artist's Related Artists

Retrieve a list of artists that are similar to a specific artist based on Spotify users' listening history.

**Endpoint:** `/artists/{id}/related-artists`


### Request

**HTTP Method:** `GET`

| **Parameter**   | **Necessity**   | **Type**  | **Description**  | **Example values**  |
|--------------|-----------------|----------------|----------------|----------------|
| `id` | Required | String | The Spotify ID of the artist. | `4k1ELeJKT1ISyDv8JivPpB`


### Response – Success

When the response has been successfully completed, a `200 OK` response is returned along with the information listed in the following table.

| **Parameter**   |  **Type**  | **Description**  | **Additional data included** |
|--------------|-----------------|----------------|----------------|
| `artists` | Array of ArtistObjects | A list of the related artists. | `external_urls`: External URL of the artist. </br>`followers`: Information about the artist's followers. </br>`genres`: The list of genres associated with the artist. For example: `["Punk", "Hardcore"]` </br>`href`: A link to the endpoint that provides full details on the artist. </br>`id`: The Spotify ID of the artist.</br>`images`: Images of the artist. </br>`name`: The name of the artist. </br>`popularity`: The popularity of the artist on a scale between *0* (the least popular) and *100* (the most popular). </br>`type`: The object type; always `artist` for this response type.</br>`uri`: The Spotify URI of the artist. |


### Response – Error

When the response cannot be successfully completed, one of the following error codes is returned.

| **Response**   | **Description**   | **Solution**  | 
|--------------|-----------------|-----------------|
| `401 Unauthorized` | Invalid or expired token. | Re-authenticate. |
| `403 Forbidden` | Request refused by the server. | Do not repeat the request. |
| `429 Too Many Requests` | Too many requests from the user's IP. | Repeat the request later. |