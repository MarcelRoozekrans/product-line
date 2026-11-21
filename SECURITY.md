# Security policy

This policy applies to every product in the line. A product's own `SECURITY.md` may add to it but not weaken it. The decision behind it is [ADR-248](docs/adr/0248-publish-a-vulnerability-disclosure-policy-for-every-product.md).

## Reporting a vulnerability

Please do not open a public issue. Report privately through GitHub: open the affected repository, go to the **Security** tab and choose **Report a vulnerability**.

Include what you found, the affected product and version, and the steps to reproduce it.

## What happens next

* You get a confirmation of receipt within 5 working days.
* You get a first assessment, including severity, within 15 working days.
* The aim is to fix the issue and disclose it together with you within 90 days, sooner for critical findings.
* You are credited in the advisory if you want to be.

## Supported versions

* The latest release of each product receives security fixes.
* After a new major version, the previous major still receives fixes for critical and high findings for 6 months. After that, only the latest release.

## Good-faith research

Research that follows this policy is welcome. Do not access or change data that is not yours, do not degrade service for others, and give us the time above before you disclose.

## Out of scope

* Third-party services the products connect to.
* Volumetric denial of service.
* Bypassing a guardrail on your own machine, where you already control the process.
