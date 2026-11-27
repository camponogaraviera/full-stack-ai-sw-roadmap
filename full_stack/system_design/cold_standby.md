<div align='center'>
    <h1> Resiliency & High Availability </h1>
    <h2> Cold Standby </h2>
</div>

# Table of Contents

- [Introduction](#introduction)
- [References](#references)

# Introduction

Data from the primary database is backed up/copied to a standby database that is offline or not continuously synchronized. The standby must be started/prepared before it can take over.

Its primary purpose is disaster recovery.

---

# References

[1] https://docs.aws.amazon.com/whitepapers/latest/disaster-recovery-workloads-on-aws/disaster-recovery-options-in-the-cloud.html

[2] https://www.ibm.com/docs/en/wam/wm-api-gateway/10.11.0?topic=recovery-what-is-cold-standby-mode
