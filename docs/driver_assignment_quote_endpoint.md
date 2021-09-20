## Driver Authorisation Request

The driver authorisation request object is required to be created first before a driver authorisation can be created.

### Create Driver Authorisation Request

In order to create a driver authorisation request you must have created a [driver](./driver_endpoint.md) and a [vehicle](./vehicle_endpoint.md) first and have an active [policy](./policy_endpoint.md).

When creating the authorisation request, Zego checks that the driver and vehicle combination meet the underwriting criteria defined on the policy. It also calculates any charges that might occur from the creation of the authorisation. Wether or not an authorisation will have charges depends on the product offering you have agreed with Zego.

Once a request has been made and it has a status of **approved**, you can confirm it to create an authorisation. See [driver authorisation](./driver_authorisation_endpoint.md). You may also decide that you do not wish no confirm it.

It will not be possible to confirm authorisation requests that have reached their expiry time. You will need to request a new one if this happens.

`POST /v2/fleet/driver-authorisation-request/`

#### Request Body

| Key | Type | Required | Notes |
| --- | --- | --- | --- |
| policyId | string | yes |  |
| fleetDriverId | string | yes |  |
| fleetVehicleId | string | yes |  |
| startTime | iso-8601 date time | no |  |
| endTime | iso-8601 date time | no |  |

**startTime** and **endTime** may be required depending on the policy criteria. For example, we may offer different rates based on the length of the authorisation. If this is the case you will need to provde the **startTime** and **endTime** otherwise the request will be declined.

##### Example body

```
{
    "policyId": "fltpol_bieoesnrpfftvicp2d7xbiratq",
    "fleetDriverId": "fltdrv_kiaqgehibbhktagg75drzcssxy",
    "fleetVehicleId": "fltveh_hhz2wvyhdrgnzlt3pc2li24xmm",
    "startTime": "2019-08-01T17:00:00",
    "endTime": "2019-12-01:21:00:00"
}
```

##### Example response

HTTP 201

```
{
  "id": "fltdar_btoowm25rzdljdm6ln7pon6yry",
  "fleetVehicleId": "fltveh_hhz2wvyhdrgnzlt3pc2li24xmm",
  "fleetDriverId": "fltdrv_kiaqgehibbhktagg75drzcssxy",
  "status": "approved",
  "rates": [
    {
      "kind": "premium_per_day",
      "amount": 992,
      "currency": "GBP"
    },
    {
      "kind": "excess",
      "amount": 50000,
      "currency": "GBP"
    }
  ],
  "expireTime": "2022-07-07T12:00:00+00:00",
}
```

##### Example error

HTTP 401

```
{
  "status": "NOK",
  "error": "INVALID_DATA",
  "message": "Invalid data format",
  "detail": {
    "fleetDriverId": [
    "`id` does not exist."
    ]
  }
}
```

### Get Driver Authorisation Request

`GET /v2/fleet/driver-authorisation-request/:id`

##### Example response

HTTP 201

```
{
  "id": "fltdar_btoowm25rzdljdm6ln7pon6yry",
  "fleetVehicleId": "fltveh_hhz2wvyhdrgnzlt3pc2li24xmm",
  "fleetDriverId": "fltdrv_kiaqgehibbhktagg75drzcssxy",
  "status": "approved",
  "rates": [
    {
      "kind": "premium_per_day",
      "amount": 992,
      "currency": "GBP"
    },
    {
      "kind": "excess",
      "amount": 50000,
      "currency": "GBP"
    }
  ],
}
```

### List Driver Authorisation Requests

`GET /v2/fleet/driver-authorisation-request/`

##### Example response

HTTP 201

```
{
  "requests": [
    {
      "status": "declined",
      "fleetVehicleId": "fltveh_hhz2wvyhdrgnzlt3pc2li24xmm",
      "fleetDriverId": "fltdrv_kiaqgehibbhktagg75drzcssxy",
      "id": "fltdar_btoowm25rzdljdm6ln7pon6yry",
      "rates": null,
      "expireTime": null,
    },
    {
      "declined": "approved",
      "fleetVehicleId": "fltveh_hhz2wvyhdrgnzlt3pc2li24xmm",
      "fleetDriverId": "fltdrv_kiaqgehibbhktagg75drzcssxy",
      "id": "fltdar_btoowm25rzdljdm6ln7pon6yry",
      "rates": null,
      "expireTime": "2019-12-01:21:00:00",
    }
  ]
}
```