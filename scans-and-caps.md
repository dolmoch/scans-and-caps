# Scans and caps

The goal of this game is to build judgment around network services, attack surface, and practical risk reduction.

You will:

1. predict which network services you expect to find on a device and investigate what is actually there;
2. turn those observations into plausible risk stories;
3. propose **risk caps**: practical measures that reduce a risk enough to live with it, followed by more durable fixes where appropriate.

The goal is **not** to find as many vulnerabilities as possible or to exploit anything. The goal is to reason about risk from imperfect evidence.

You do not need to know port numbers or services from memory. References, documentation, search, and AI are all fair game. The important part is that you make your current hypothesis visible before asking a tool for an answer, then decide what the result actually supports.

## Getting feedback

The easiest way to get feedback or help is to use an AI assistant.

Upload this document together with your report and ask, for example:

> Provide feedback on my report for this game. Look especially for conclusions that are stronger than the evidence.

Or, if you are stuck:

> I am stuck at [point] in this game. Help me identify useful next tests without solving the investigation for me.

Strong reports generally:

- state a prediction or hypothesis before running a tool, consulting a resource, or searching for information;
- interpret the evidence instead of treating tool output as a conclusion;
- use contradictory or surprising results for further inquiry;
- preserve unknowns when they cannot yet be resolved;
- stay in control of the investigation.

Weak reports tend to:

- run tools or collect information and treat their output as decisive;
- select only evidence that confirms the original idea;
- present polished information they cannot explain or use two steps later.

## Requirements and boundaries

You need Nmap or a similar network scanner.

Scan devices you own or devices whose owner has explicitly given you permission to scan. This exercise only requires service discovery and ordinary interaction with those services; exploitation, brute force, or intrusive scanning is outside its scope.

You will also need the IP address of each target device. If you do not know it, your router's connected-device list or the device's own network settings are usually good places to look.

A scan is a sample of what you can observe from one place, using one method, at one moment. Not seeing a service is therefore not proof that no service exists.

# Level 1: What's on the network?

**Goal:** Build confidence and pattern recognition while learning to separate prediction from observation.

Pick two devices such as a router, laptop, phone, TV, printer, or NAS.

For each device:

### 1. Predict

Before scanning, write what you expect.

For example:

> I expect this printer to expose a web interface because it can be configured through a browser. I also expect a printing service, but I do not know which protocol it uses.

### 2. Measure

Scan the device:

```text
nmap <ip-address>
```

Record the open ports you find.

You do not have to investigate every port if the device exposes many services. Select a manageable set of interesting or relevant services and record the rest as unresolved.

### 3. Adjust

Compare the result with your prediction.

For example:

> I expected ports X and Y. X was present, but Y was not. I also found Z, which I did not expect. My next hypotheses are...

A port number is not a conclusion. It is another clue.

If port 80 is open, for example:

1. **Predict:** “Port 80 may be a web interface for administering or monitoring this device.”
2. **Measure:** “I will open it in a browser and inspect what the service presents.”
3. **Adjust:** “It serves a status page with no obvious administration controls. That makes my original admin-interface hypothesis less likely, but does not rule it out. I will now check…”

Verification can involve service detection, opening the service with an appropriate client, consulting device documentation, checking the device's own settings, or another suitable test.

It is perfectly acceptable to finish with:

> I have not yet established what this service is.

That is better than converting an uncertain label into a fact.

## Minimum deliverable

For two devices, record:

- your predictions;
- actual open ports;
- important surprises or contradictions.

For the services you investigate, record:

- likely service;
- verification steps and evidence;
- intended purpose;
- remaining uncertainty, if any.

# Level 2: Understand attack surface and risk

**Goal:** Move from observations to risk stories.

Choose several of the services you investigated in Level 1.

For each one, ask:

> What could happen to this service that would endanger what the device is supposed to do?

Describe one or two plausible failure modes and label their main impact:

- **Confidentiality** — information becomes available to someone who should not have it.
- **Integrity** — someone can make an unauthorized change.
- **Availability** — the service or device stops being usable when it is needed.

Prefer a small concrete story over a vulnerability label.

For example:

> **Availability:** An attacker or malfunctioning client could submit enough print jobs to make the printer unavailable for legitimate users.

Or:

> **Integrity:** If the administration interface allowed unauthorized access, someone could change the printer's network configuration and redirect or disrupt its traffic.

You do not need to prove that the attack is currently possible. At this stage you are identifying a plausible failure mode worth reasoning about.

# Level 3: Cap the risks

**Goal:** Turn a risk story into a decision: what would you do Monday morning?

For each failure mode, propose four things.

### Fast cap

What could reduce the exposure quickly?

Examples:

- disable an unnecessary service;
- restrict access to the local network;
- add a firewall rule;
- move an IoT device onto a guest or isolated network;
- replace a default password.

A fast cap does not need to eliminate the risk. Its purpose is to reduce it cheaply while you decide whether anything more is justified.

### Durable fix

What would address the problem more permanently?

Examples:

- update or replace vulnerable firmware;
- rotate credentials or keys;
- redesign network segmentation;
- replace an obsolete service;
- introduce appropriate monitoring or logging.

### Verification

What evidence would tell you later that the cap is still working?

Try to test the control itself.

For example:

> I restricted the administration service to the trusted LAN. Once a month I will attempt to reach it from the guest network; that connection should fail.

### Assumption and revisit trigger

What must remain true for your decision to make sense?

And what change would make you reconsider it?

For example:

> I assume this service only needs to be reachable from the home LAN. I would revisit the decision if remote access became a requirement.

Other revisit triggers might include:

- confidential data being added;
- the device gaining Internet exposure;
- a new automation or integration;
- a firmware change;
- a new vulnerability affecting the service;
- a change in how important the device is.

# Finished?

A strong result is not the report with the most ports, vulnerabilities, or controls.

It is one where another person can see:

**what you expected → what you observed → what you think it means → what remains uncertain → what you would do about it.**

If those transitions are inspectable, the game has done its job.
