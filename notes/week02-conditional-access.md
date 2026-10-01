# Conditional Access Study Notes

## 1. What is Conditional Access?

Conditional Access is a Microsoft Entra security feature used to control access based on specific signals and conditions.

The basic idea is:

**Who + What + Conditions = Access decision**

Conditional Access can decide whether to:

- Allow access
- Block access
- Require MFA
- Require a compliant device
- Require a specific authentication strength
- Apply session controls

Conditional Access acts like a security gate between the user and the resource they are trying to access.

---

## 2. Main Conditional Access Building Blocks

A Conditional Access policy usually includes:

### Users or groups
Defines who the policy applies to.

Examples:
- Contractors
- Administrators
- Finance users
- Security analysts

### Target resources
Defines what the user is trying to access.

Examples:
- Microsoft 365
- Azure Management
- A specific enterprise application
- All resources

### Conditions
Defines when the policy should apply.

Examples:
- Device platform
- Location
- Client application
- Device state
- Sign-in risk
- User risk

### Grant controls
Defines what the user must do to gain access.

Examples:
- Require MFA
- Require compliant device
- Require hybrid joined device
- Require authentication strength
- Block access

### Session controls
Defines how the session behaves after access is granted.

Examples:
- Sign-in frequency
- Persistent browser session
- App enforced restrictions

---

## 3. Conditional Access Logic

Example:

A contractor accesses company resources.

Policy logic:

- User: Contractor
- Target resource: Company resources
- Condition: Any sign-in
- Grant control: Require MFA

In simple terms:

**If a contractor signs in to company resources, require MFA.**

---

## 4. Why Security Groups Are Useful with Conditional Access

Conditional Access policies can be applied to groups instead of individual users.

For example:

`SG-Contractors`

Instead of adding every contractor individually to a Conditional Access policy, the policy can target the whole contractor security group.

This makes access management easier and more scalable.

If a contractor leaves the organization, removing them from the group can remove their access to policies and resources associated with that group.

---

## 5. Break-Glass / Emergency Account

A break-glass account is an emergency administrator account used when normal administrator accounts cannot sign in.

It is similar to having an emergency spare key.

The account should normally:

- Not be used for daily work
- Have a strong unique password
- Be monitored
- Be kept separate from normal admin accounts
- Be excluded from Conditional Access policies that could cause lockout

In the lab, I created:

`emergency.admin@otmcorp964.onmicrosoft.com`

This account was assigned the Global Administrator role for emergency recovery purposes.

The account was excluded from the Conditional Access policy.

---

## 6. Why the Break-Glass Account Was Excluded

If a Conditional Access policy is misconfigured, it could accidentally lock out administrators.

By excluding the emergency administrator account, there is still a way to access the tenant and fix the policy.

This is especially important when testing policies involving:

- MFA
- Device compliance
- Location restrictions
- Authentication strength
- Block access

---

## 7. First Conditional Access Policy

### Policy Name

`CA - Contractors-Require-MFA`

### Purpose

Require contractors to use multifactor authentication when accessing company resources.

### Users

Include:

`SG-Contractors`

Exclude:

`Emergency Admin`

### Target resources

`All resources`

### Network

Not configured.

Network conditions were not needed because the requirement was to require MFA for contractors regardless of where they signed in from.

### Conditions

None selected.

No conditions were required because this policy should apply to all contractor sign-ins.

### Grant Control

Grant access with:

`Require multifactor authentication`

### Policy State

`Report-only`

---

## 8. Why Report-Only Mode Was Used

Report-only mode allows a Conditional Access policy to be evaluated without actually enforcing it.

This makes it safer to test policies.

Instead of immediately requiring MFA or blocking access, Entra records what would have happened if the policy were enabled.

This helps prevent accidental user or administrator lockouts.

Recommended workflow:

1. Create the policy
2. Set it to Report-only
3. Test sign-ins
4. Review Conditional Access results
5. Fix any issues
6. Enable the policy only after testing

---

## 9. Testing the Conditional Access Policy

Qasim Bashir was used as the test user.

Qasim is a member of:

`SG-Contractors`

A separate browser session was used to sign in as Qasim.

Qasim successfully accessed his Microsoft account.

Because the Conditional Access policy was in Report-only mode, MFA was not actually enforced during the sign-in.

---

## 10. Reviewing Sign-In Logs

After Qasim signed in, the sign-in event appeared in:

**Microsoft Entra Admin Center → Sign-in logs**

The sign-in event was opened and the following tabs were reviewed:

- Basic info
- Location
- Device info
- Authentication details
- Conditional Access
- Report-only

The standard Conditional Access tab showed:

`Not applicable`

This happened because the policy was not actively enforced.

The policy was instead visible under the:

`Report-only`

tab.

---

## 11. Report-Only Result

The following policy appeared in the Report-only tab:

`CA - Contractors-Require-MFA`

Grant control:

`Require multifactor authentication`

Result:

`Report-only: User action required`

This confirmed that the Conditional Access policy successfully matched Qasim's sign-in.

If the policy had been enabled, Qasim would have been required to complete MFA.

---

## 12. What Was Learned from the Test

The test demonstrated that:

- Conditional Access policies can target security groups
- Emergency accounts can be excluded
- Policies can target all resources
- MFA can be required through grant controls
- Conditions are optional depending on the use case
- Report-only mode is useful for safe testing
- Sign-in logs show whether a policy matched
- Report-only policies are reviewed under the Report-only tab
- Conditional Access policies should be tested before being enabled

---

## 13. Network Conditions

The Network section was left as:

`Not configured`

This was intentional.

Network or location conditions are only needed when the policy requirement depends on where the user is signing in from.

Example:

- Inside trusted office network → allow normally
- Outside trusted network → require MFA

For the contractor MFA policy, MFA was required regardless of location, so no network condition was necessary.

---

## 14. Conditions

Conditions were also left unconfigured.

Conditions can be used to make policies more specific.

Examples include:

- Device platform
- Location
- Client apps
- Device state
- Sign-in risk
- User risk

For the contractor policy, the requirement was simple:

**Any contractor accessing company resources should require MFA.**

Therefore, no additional condition was required.

---

## 15. Conditional Access vs Security Defaults

During testing, the sign-in log also showed Security Defaults being evaluated.

Security Defaults provides basic identity protection such as MFA enforcement with minimal configuration.

Conditional Access provides more detailed and customizable control.

Conditional Access can target:

- Specific users
- Specific groups
- Applications
- Locations
- Devices
- Client apps
- Authentication methods
- Risk levels

Security Defaults is simpler.

Conditional Access is more flexible and is commonly used in organizations that need detailed access control.

---

## 16. Conditional Access Licensing

Conditional Access requires Microsoft Entra ID P1 or higher.

The lab originally used Entra Free, which did not allow Conditional Access policy creation.

A Microsoft Entra ID P1 trial was later activated.

This enabled:

- Conditional Access policy creation
- Report-only testing
- MFA grant controls
- Location and device-based conditions

Risk-based Conditional Access features such as user risk and sign-in risk require Microsoft Entra ID P2.

---

## 17. Key Takeaways

The main Conditional Access workflow is:

**Who**
→ users or groups

**What**
→ applications or resources

**Conditions**
→ location, device, client app, risk, etc.

**Control**
→ MFA, compliant device, authentication strength, or block

Example:

`SG-Contractors`
→ `All resources`
→ `Any sign-in`
→ `Require MFA`

Always test important policies in Report-only mode before enabling them.

Always maintain emergency access so administrators do not accidentally lock themselves out.
