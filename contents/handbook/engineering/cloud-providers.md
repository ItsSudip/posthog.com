---
title: Working with cloud providers
sidebar: Handbook
showTitle: true
---

## AWS

### How do I get access?

Ask in the `#team-platform` Slack channel for someone to add you. Anyone who has edit access can do so.

To give someone access (important to follow these steps!):
1. navigate to [IAM](https://console.aws.amazon.com/iamv2/home#/users)
2. Use the `Add Users` button at the top right to add their PostHog email as username and select 'password'.
3. Important! Untick 'User must create a new password at next sign-in', as that'll cause issues with MFA.
4. Pick `AWS Management Console access` and add them to the `Team` group
5. Slack the username, password and console link to new user.
6. User has to log in, set up MFA, log out, and then change password. They may need to log out again.

#### IMPORTANT: You must log out and then log back in!

This is because of MFA. Just adding MFA isn't quite enough for AWS console so if you are seeing a bunch of really weird permissions errors. Log out, then log back in using your MFA token. This should resolve everything!

### Permissions errors using aws cli

If you see something like:
```
<my-user> is not authorized to perform: <action> on resource: <resource> with an explicit deny
```

Note the "with an explicit deny" in the end which likely is due to the fact that we force Multi-Factor Authentication (MFA). Follow [this guide](https://aws.amazon.com/premiumsupport/knowledge-center/authenticate-mfa-cli/) to use a session token. 

TLDR:

1. Look up your security credential MFA device name from AWS console from `https://console.aws.amazon.com/iam/home#/users/<user-name>?section=security_credentials`
2. Run `aws sts get-session-token --serial-number <arn-of-the-mfa-device> --token-code <code-from-token> --duration 129600` where `code-from-token` is the same code you'd use to login to the AWS console (e.g. from Authy app).
3. Run the following code, replacing the placeholder values with the appropriate ones:
```
export AWS_ACCESS_KEY_ID=example-access-key-as-in-previous-output
export AWS_SECRET_ACCESS_KEY=example-secret-access-key-as-in-previous-output
export AWS_SESSION_TOKEN=example-session-token-as-in-previous-output
```
4. Unset them when done (after they expire before running `get-session-token` again):
```
unset AWS_ACCESS_KEY_ID && unset AWS_SECRET_ACCESS_KEY && unset AWS_SESSION_TOKEN
```

### Deploying PostHog

See docs [here](https://posthog.com/docs/self-host/deploy/aws).


## GCP

### How do I get access?

Ask someone in the `#team-platform` Slack channel to add you. Anyone who has edit access can do so.

To give someone access: Navigate to [PostHog project IAM](https://console.cloud.google.com/iam-admin/iam?project=posthog-301601&supportedpurview=project) and use the `+Add` button at the top to add their PostHog email address and toggle `Basic` -> `Editor` role.

### Deploying PostHog

See docs [here](https://posthog.com/docs/self-host/deploy/gcp).


## DigitalOcean

### How do I get access?

Ask in the `#team-platform` Slack channel for someone to add you. Anyone who is an owner can do so.

To give someone access: Navigate to [PostHog team settings page](https://cloud.digitalocean.com/account/team?i=7cfa7c) and use the `Invite Members` button to add their PostHog email address.

### Edit 1-Click app info

This can be done in the [vendor portal](https://cloud.digitalocean.com/vendorportal/), click on `PostHog` with Approved status to edit the listing.

The code and setup files are in [digitalocean/marketplace-kubernetes repository](https://github.com/digitalocean/marketplace-kubernetes/tree/master/stacks/posthog).

### Deploying PostHog

See docs [here](https://posthog.com/docs/self-host/deploy/digital-ocean).
