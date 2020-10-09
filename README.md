# oscp-geopose-protocol
OSCP GeoPose Protocol

## Purpose

Early version of a standard GeoPose API i.e. a request/response protocol for visual localization. The GeoPose representation will be formalized through the [OGC GeoPose Working Group](https://www.ogc.org/projects/groups/geoposeswg).


## GeoPose Protocol Request (version 1)


```js
export interface CameraParam {
  model?: string; //UNKNOWN_CAMERA, SIMPLE_PINHOLE, SIMPLE_RADIAL, RADIAL, PINHOLE, OPENCV, FULL_OPENCV
  modelParams?: number[];
  minMaxDepth?: number[]; // for depth image
  minMaxDisparity?: number[]; // for disparity image
}

export interface Privacy {
  dataRetention: string[]; //acceptable policies for server-side data retention
  dataAcceptableUse: string[]; //acceptable policies for server-side data use
  dataSanitizationApplied: string[]; //client-side data sanitization applied
  dataSanitizationRequested: string[]; //server-side data sanitization requested
}

export interface CameraReading {
  sequenceNumber: number;
  imageFormat: string; //ex. RGBA32, GRAY8, DEPTH
  size: number[]; //width, height
  imageBytes: string; //base64 encoded data
}

//aligns with https://w3c.github.io/geolocation-sensor/
export interface GeolocationReading {
  latitude: number;
  longitude: number;
  altitude: number;
  accuracy: number;
  altitudeAccuracy: number;
  heading: number;
  speed: number;
}

export interface WiFiReading {
  BSSID: string;
  frequency: number;
  RSSI: number;
  SSID: string;
  scanTimeStart: Date;
  scanTimeEnd: Date;
}

export interface BluetoothReading {
  address: string;
  RSSI: number;
  name: string;
}

export interface Sensor {
  id: string;
  name?: string;
  model?: string;
  rigIdentifier?: string
  rigRotation?: number[] //rotation quaternion from rig to sensor
  rigTranslation?: number[] //translation vector from rig to sensor
  type: string; //camera, geolocation, wifi, bluetooth
  params?: CameraParam
}

export interface SensorReading {
  timestamp: Date;
  sensorId: string;
  privacy: Privacy;
  reading?: (CameraReading | GeolocationReading | WiFiReading | BluetoothReading)
}

export interface GeoPose {
  longitude: number;
  latitude: number;
  ellipsoidHeight: number;
  quaternion: number[];
}

export interface GeoPoseResp {
  id: string;
  timestamp: Date;
  accuracy: number;  
  type: string; //ex. geopose
  pose: GeoPose; 
}

export interface GeoPoseReq {
  id: string;
  timestamp: Date;
  type: string; //ex. geopose
  sensors: Sensor[];
  sensorReadings: SensorReading[];
  priorPose?: GeoPoseResp[]; //previous geoposes
}
```

## GeoPose Protocol Response (version 1)


```js
export interface GeoPose {
  longitude: number;
  latitude: number;
  ellipsoidHeight: number;
  quaternion: number[];
}

export interface GeoPoseResp {
  id: string;
  timestamp: Date;
  accuracy: number;  
  type: string; //ex. geopose
  pose: GeoPose; 
}
```

## Exclusions (version 1)

- streaming
- video
- other sensor types ex. LIDAR
- point cloud
- image features

