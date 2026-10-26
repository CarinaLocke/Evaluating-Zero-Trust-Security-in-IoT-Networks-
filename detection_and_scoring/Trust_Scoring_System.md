# IoT Trust-Scoring System

## Project: Evaluating Zero-Trust Security in IoT Networks
**Team Member:** Bryan  
**Responsibility:** Detection and Trust Scoring

## 1. Purpose

The purpose of this trust-scoring system is to evaluate IoT devices
based on their identity, authorization, and network behavior.
Each device receives positive or negative points depending on
its activity. The total score determines whether the device is
Trusted, Suspicious, or Untrusted.

This scoring system will help us identify unauthorized devices
and suspicious network activity in our simulated IoT network.

## 2. Trust-Scoring Rules

We will use the following scoring rules based on the professor's
project guidelines.

| Condition | Score |
|---|---:|
| Registered device | +2 |
| Normal behavior | +2 |
| Authorized destination | +1 |
| Unknown device | -3 |
| Abnormal traffic | -2 |
| Unauthorized access | -3 |

Positive points are awarded when a device is registered, behaves
normally, or communicates with an authorized destination.

Negative points are assigned when a device is unknown, shows
abnormal network behavior, or attempts unauthorized access.

Only conditions supported by the available device information
and test observations will be scored.

## 3. Device Classification

After calculating the total score, each device will be placed
into one of three categories.

| Total Score | Classification |
|---|---|
| 3 or higher | Trusted |
| 0 to 2 | Suspicious |
| Below 0 | Untrusted |

- **Trusted:** The device meets the expected security conditions.
- **Suspicious:** The device shows some security concerns and may
  require additional verification.
- **Untrusted:** The device shows significant security concerns
  and should be restricted or blocked according to group policy.

These thresholds are proposed by our group for the initial
testing stage and may be adjusted based on the results.

## 4. Trust-Scoring Examples

### Example 1: Normal Motion Sensor

A registered motion sensor communicates normally with its
authorized IoT server.

| Condition | Score |
|---|---:|
| Registered device | +2 |
| Normal behavior | +2 |
| Authorized destination | +1 |
| **Total** | **5** |

**Classification: Trusted**

The motion sensor receives a score of 5 because it is registered,
behaves normally, and communicates with an approved destination.

### Example 2: Unknown Device

An unknown device attempts to access a network resource
without authorization.

| Condition | Score |
|---|---:|
| Unknown device | -3 |
| Unauthorized access | -3 |
| **Total** | **-6** |

**Classification: Untrusted**

The device receives a score of -6 because it is not registered
and attempts unauthorized access.

### Example 3: Registered Device With Abnormal Traffic

A registered IoT device generates abnormal traffic but has
not been observed attempting unauthorized access.

| Condition | Score |
|---|---:|
| Registered device | +2 |
| Abnormal traffic | -2 |
| **Total** | **0** |

**Classification: Suspicious**

The device receives a score of 0 because it is registered
but shows abnormal network behavior.

## 5. How the Scoring System Will Be Used

During testing, we will record the behavior of each IoT device
and assign points based on the scoring rules.

The total score will determine whether the device is Trusted,
Suspicious, or Untrusted.

We will compare these classifications with the expected
results from our controlled test scenarios to measure how
accurately the system identifies suspicious activity.

The results will help us calculate detection rates and
false-positive rates.

## 6. Next Steps

Our next step is to create an Excel spreadsheet that
automatically calculates trust scores and classifies devices.

We will then use Carina's network test results to evaluate
the scoring system.

Note: Device authorization rules will be confirmed with
Nephie before final testing. The examples above demonstrate
the scoring method and are not actual experimental results.
