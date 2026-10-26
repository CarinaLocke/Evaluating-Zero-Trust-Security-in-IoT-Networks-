# Normal and Abnormal IoT Device Behavior

## Project: Evaluating Zero-Trust Security in IoT Networks
**Team Member:** Bryan
**Responsibility:** Detection and Trust Scoring

## 1. Purpose

The purpose of this section is to identify normal and abnormal
behavior in our simulated IoT network. By understanding how each
device should communicate, we can identify suspicious activities
and determine whether a device should be trusted.

## 2. Normal Device Behavior

Normal behavior occurs when an authorized IoT device communicates
with its approved server using permitted network services.

| Device | Normal Behavior |
|---|---|
| Motion Sensor | Communicates with IoT Server (192.168.30.10) |
| Smart Camera | Communicates with Video Server (192.168.30.20) |
| Thermostat | Communicates with IoT Server (192.168.30.10) |
| Door Lock | Communicates with IoT Server (192.168.30.10) |

## 3. Abnormal Device Behavior

Abnormal behavior occurs when an unknown device attempts to
access the network or an authorized device communicates with
an unauthorized destination.

| Device | Abnormal Behavior |
|---|---|
| Motion Sensor | Attempts to access the Video Server |
| Smart Camera | Attempts to access the IoT Server |
| Thermostat | Attempts to access an unauthorized server |
| Door Lock | Attempts to access an unauthorized destination |
| Rogue Device | Attempts to access network resources without authorization |

## 4. Detection Criteria

Our detection system will evaluate devices based on:

1. Device identity and registration.
2. Authorization to access network resources.
3. Expected network behavior.
4. Communication with approved destinations.
5. Suspicious or unauthorized activity.

These factors will be used to calculate a trust score for each
device. Devices will eventually be classified as Trusted,
Suspicious, or Untrusted.

## 5. Next Steps

The next stage is to create a trust-scoring system that assigns
positive or negative points based on device behavior.

We will use the scoring system to identify suspicious devices
and evaluate detection accuracy.

Note: Device communication rules are based on the current network
documentation and will be confirmed with the team's final
authorization policies.
