<div align='center'>
    <h1> Resiliency & High Availability </h1>
    <h2> Warm Standby </h2>
</div>

# Table of Contents

- [Introduction](#introduction)
- [References](#references)

# Introduction

Data from the primary database is replicated periodically to a standby system, but the standby may not be fully up-to-date or immediately ready to serve traffic. If the primary fails, the standby is brought online, and traffic is redirected to it.

Its primary purpose is disaster recovery and high availability.

---

# References

[1] https://docs.aws.amazon.com/whitepapers/latest/disaster-recovery-workloads-on-aws/disaster-recovery-options-in-the-cloud.html#warm-standby
