---
seo:
  title: HPE GreenLake Developer Role and Permission Naming Specification | HPE GreenLake Platform
toc:
  enable: true
---

# HPE GreenLake Developer Role and Permission Naming Specification

This specification outlines the importance of predefined roles and permissions,
emphasizing consistency in naming, syntax, and semantics to enhance customer
understanding and improve their security posture. It provides guidelines for
permission granularity, supporting the best practice of least privilege, and
aims to reduce internal friction by establishing clear standards, ensuring teams
make correct choices from the outset.

## Standard Scope

The standard for programmatic, machine-consumable role and permission
names applies to the values exposed via workspaces with GLP
Authorization v2 enabled and via the GLP Authorization v2 service's
APIs.

The guidance for display names and descriptions applies to both AuthZ v1
and AuthZ v2 registration.

## Concise and Consistent Names for Roles and Permissions

The standards in this section apply to both role names and permission
strings.

- Names SHOULD be concise. Users will often browse a large list of
  names, and overly verbose names are difficult to scan.

- Names MUST NOT use abbreviations except as described below. Names MUST
  use full words instead of acronyms to improve comprehension. Two
  exceptions are allowed:

  - Common industry acronyms are acceptable when they improve
    readability for our customers (e.g., IP addresses vs. Internet
    Protocol addresses).

  - Other abbreviations may be allowed to reduce very long names. These
    exceptions MUST be reviewed by the HPE GreenLake platform UX team.

- Names SHOULD align with the backend API names. This way, customers can
  switch between UI-driven and API-driven operations easily.

- Names MUST use customer-friendly names suitable for public display.
  Names MUST NOT use internal code names.

  - Example: Use `managed-private-cloud` instead of the internal code
    name `agena`.

## Permission Standards

### Scope of Standard

This standard applies to all permissions registered to the GreenLake
Platform, including public and internal permissions.

### Permission Attribute Requirements

<table>
<colgroup>
<col style="width: 12%" />
<col style="width: 36%" />
<col style="width: 32%" />
<col style="width: 18%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>Attribute</strong></th>
<th style="text-align: center;"><strong>Definition</strong></th>
<th style="text-align: center;"><strong>Requirements</strong></th>
<th style="text-align: center;"><strong>Usage</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="vertical-align: top;"><p>Name</p></td>
<td style="vertical-align: top;"><p>A human-friendly permission name, typically following this
format:</p>
<p><code>&lt;provider&gt;.&lt;resource&gt;.&lt;action&gt;</code>
</p></td>
<td style="vertical-align: top;"><ul>
<li><p>Name MUST conform to <a href="#dot-notation-format">dot notation format</a>.</p></li>
<li><p>Name MUST use <code>a-z</code> and <code>-</code> (hyphen) only.</p></li>
<li><p>Multiple words in a segment MUST use - as the word separator.</p>
<ul>
<li><p>Example: <code>service.sso-profile.update</code></p></li>
</ul></li>
</ul></td>
<td style="vertical-align: top;"><ul>
<li><p>The canonical identifier of the permission.</p></li>
<li><p>Displayed in the web UI.</p></li>
<li><p>Used in APIs.</p></li>
</ul></td>
</tr>
<tr>
<td style="vertical-align: top;"><p>Description</p></td>
<td style="vertical-align: top;"><p>Friendly, human-readable description of the permission.</p></td>
<td style="vertical-align: top;"><ul>
<li><p>MUST be defined for each permission.</p></li>
<li><p>MUST be a complete sentence.</p></li>
<li><p>MUST use sentence case and punctuation.</p></li>
<li><p>MUST provide additional, useful information about the
privilege.</p></li>
<li><p>MUST not be only a restatement of the permission action, e.g.,
“Delete permission”.</p></li>
<li><p>MUST accurately and exhaustively explain what the permission allows. If
this results in a lengthy description, the permission may be trying to do too
much; consider splitting it into multiple permissions.</p></li>
<li><p>When the permission action is <code>update</code>, if the permission grants
additional privileges beyond updating object attributes (such as
“power-on server”), the description MUST explicitly describe the actions
granted.</p></li>
<li><p>SHOULD be short. This description will be displayed in the UI
when selecting or viewing permissions and must be easy and quick for a
user to consume and short so it can be displayed in UI selectors.
Wrapping of a permission description can start around 80
characters.</p></li>
<li><p>MUST be concise. Do not be overly wordy in permission
descriptions. Make them as simple and clear as possible.</p></li>
<li><p>SHOULD avoid the word “permission”. For example, starting every
description with “Permission to…” is redundant when viewing a list of
permissions.</p></li>
<li><p>SHOULD be reviewed with a focus on how it will be displayed in
the HPE GreenLake Console UI, especially within the Create Custom Role
flow.</p></li>
</ul></td>
<td style="vertical-align: top;"><ul>
<li><p>Displayed in the web UI.</p></li>
</ul></td>
</tr>
</tbody>
</table>

#### Permission Description Examples

These are given as examples only. These permissions may or may not
actually exist on the platform.

| **Permission** | **Description** | **Example Notes** |
|----|----|----|
| `identity.user-group.update` | Update user group attributes |  |
| `identity.user-group.membership.update` | Update list of users in a user group |  |
| `storage.volume.delete` | Delete a volume |  |
| `compute.virtual-machine.update` | Update virtual machine configuration | In this example, the privilege to edit configuration and change power state are implemented as separate permissions. |
| `compute.virtual-machine.power-on` | Power on a virtual machine | See above. |
| `metal.server.update` | Update server attributes and set power state | In this contrasting example, power state is considered an attribute of the server and protected by a single permission for both configuration edit and power on/off. |

### Dot Notation Format

- Permissions MUST use dot notation format:

  - `<provider>.<resource>[.<subresource1>[.<subresource2>]].<action>`

- Permissions SHOULD define a single resource segment
  (`<provider>.<resource>.<action>`), unless there is a compelling
  reason to use multiple resource segments.

### Provider Segment Requirements

- Provider MUST match the Resource Provider name contributing the
  permissions.

- Provider name SHOULD match the related registered API Group name.

### Resource Segment Requirements

- Resource MUST use singular nouns.

- Resource MUST describe a specific resource. For example: `server`,
  `switch`, `sensor-firmware`, `sso-profile`.

- Resource MUST NOT use broad, generic terms unassociated with specific
  resources. Examples of terms to avoid: `global`, `system`, `service`.

### Action Segment Requirements

#### Standard Actions

The action segment SHOULD use the following Standard Actions. The action
segment MUST NOT use synonyms for the Standard Actions. E.g., actions
MUST use `read` instead of `get`, `view`, or `list`.

| **Action** | **Usage**                                                    |
|------------|--------------------------------------------------------------|
| `create`   | Create a resource                                            |
| `read`     | Read or list resources                                       |
| `update`   | Modify an existing resource or perform actions on a resource |
| `delete`   | Delete a resource                                            |

#### Custom Actions

The action segment MAY use other verbs in cases where the privilege
clearly does not align to any of the Standard Actions. When introducing
a new action segment, the action MUST be an action verb or phrase in
base form. Custom actions should align with [API standard common
actions](api/http.md#common-custom-actions) when possible.

| **Action** | **Non-Compliant Alternatives**                |
|------------|-----------------------------------------------|
| `read`     | `reads` `reader` `read-internal`              |
| `cancel`   | `withdraw`                                    |
| `move`     | `relocate`                                    |
| `suspend`  | `hold` `pause`                                |
| `resume`   | `continue`                                    |
| `apply`    | `applies` `apply-settings`                    |
| `power-on` | `powers-on` `powering-on` `turn-on-the-power` |
| `format`   | `formatter`                                   |
| `reset`    | `resets`                                      |
| `share`    | `shared`                                      |
| `use`      | `usage use-this`                              |

### Permission Granularity

#### Support Least Privilege

When designing permission granularity, it's important to consider that
IAM administrators who adhere to best practices consistently apply the
principle of least privilege. This commonly involves creating custom
roles that grant only the minimum privileges necessary for a specific
job function. To support this best practice, permissions SHOULD be
defined at the most fine-grained level that system consumers are
expected to need.

#### Guidelines for Edge Case - Overlapping Permissions

Permissions at the same hierarchical level MUST NOT overlap: One
permission MUST NOT implicitly grant privileges from another permission.
Exception: As part of an orchestrated migration from broad to granular
permissions, when the intent is to deprecate the broad permission, a new
granular permissions may overlap with the broad permission.

<table>
<colgroup>
<col style="width: 47%" />
<col style="width: 14%" />
<col style="width: 38%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>Permissions</strong></th>
<th style="text-align: center;"><strong>Standards
Evaluation</strong></th>
<th style="text-align: center;"><strong>Explanation</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="vertical-align: top;"><p><code>storage.volume.create</code></p>
<p>Allows creating volumes, but not filesystems</p>
<p><code>storage.filesystem.create</code></p>
<p>Allows creating filesystems, but not volumes</p></td>
<td style="vertical-align: top;"><p>✅ Acceptable</p></td>
<td style="vertical-align: top;"><p>These permissions do not overlap: Volume create permission does not
imply anything about filesystem permissions (including the ability to
read a filesystem). Filesystem create does not imply anything about
volume permissions.</p></td>
</tr>
<tr>
<td style="vertical-align: top;"><p><code>ccs.authorization.edit</code></p>
<p>Legacy, non-granular permission. Grants many IAM administration
privileges, including creating custom roles.</p>
<p><code>authorization.custom-role.create</code></p>
<p>New granular permission. Allows creating custom roles.</p></td>
<td style="vertical-align: top;"><p>✅ Acceptable</p></td>
<td style="vertical-align: top;"><p>Although these permissions overlap, it is part of a planned
migration to more granular permissions. The first permission will be
deprecated.</p></td>
</tr>
<tr>
<td style="vertical-align: top;"><p><code>compute.server.update</code></p>
<p>Allows updating servers, including applying firmware updates.</p>
<p><code>compute.server-firmware.update</code></p>
<p>Allows applying firmware updates to a server.</p></td>
<td style="vertical-align: top;"><p>❌ Not Acceptable</p></td>
<td style="vertical-align: top;"><p>Although the 3 segment notation is generally preferred, in this
specific situation the 3 segment notation does not accurately represent
the privilege model: Permissions are at the same hierarchical level, and the first
permission implicitly grants the second permission.</p>
<p>See next example for how to handle this case.</p></td>
</tr>
<tr>
<td style="vertical-align: top;"><p><code>compute.server.update</code></p>
<p>Allows updating servers, including applying firmware updates.</p>
<p><code>compute.server.firmware.update</code></p>
<p>Allows applying firmware updates to a server.</p></td>
<td style="vertical-align: top;"><p>✅ Acceptable</p></td>
<td style="vertical-align: top;"><p>Although these permissions overlap, the dot notation implies the
second permission is a subset of the server object. Additionally, the
first permission’s description clearly states that it includes the
privilege of the second permission.</p></td>
</tr>
<tr>
<td style="vertical-align: top;"
class="confluenceTd"><p><code>storage.system.administrator</code></p>
<p>Grants all storage system permissions (such as <code>storage.volume.create</code>)</p>
</td>
<td style="vertical-align: top;"><p>❌ Not Acceptable</p></td>
<td style="vertical-align: top;"><p>This broad permission, overlapping with other existing
permissions, is never acceptable. It effectively defines a "role". Instead, use the role
object to define a <code>Storage administrator</code> role which contains all the various
permissions required.</p></td>
</tr>
</tbody>
</table>

## Role Standards

### Scope of Standard for Roles

The standards in this document MUST be followed for predefined roles
contributed by services.

These standards SHOULD be followed for other types of roles contributed
by services, such as workspace-scoped custom roles created by services
automatically (i.e., linked roles and managed roles). We recognize that
some workspace-scoped roles may need to diverge from the guidelines. For
example, a system-generated role that combines permissions across
multiple services for a logical solution would probably not use the
service name prefix in the display name, or a linked role that maps one-to-one
with an inflexible third party integration may need to follow the
syntax of the third party system for usability.

These standards do not apply to customer-created custom roles, as the
customer is free to choose role names and descriptions within the
allowable character sets.

### Role Attribute Requirements

A role is a set of permissions. Roles are assigned to subjects and
constrained by a scope, granting the subject the ability to perform the
actions in the role on the resources targeted by the scope.

The permissions in a role dictate the privileges granted by that role.
The role name or display name MUST NOT be used directly to determine
privileges assigned to a security principal (user, API client, etc.).

<table>
<colgroup>
<col style="width: 14%" />
<col style="width: 16%" />
<col style="width: 45%" />
<col style="width: 23%" />
</colgroup>
<thead>
<tr>
<th style="text-align: center;"><strong>Role Object
Attribute</strong></th>
<th style="text-align: center;"><strong>Definition</strong></th>
<th style="text-align: center;"><strong>Requirements</strong></th>
<th style="text-align: center;"><strong>Notes</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td style="vertical-align: top;">Name</td>
<td style="vertical-align: top;">Machine usable identifier that is reasonably human readable.</td>
<td style="vertical-align: top;"><ul>
<li><p>MUST conform to <code>&lt;provider&gt;.&lt;name&gt;</code> syntax.</p></li>
<li><p><code>&lt;name&gt;</code> MUST use only <code>a-z0-9</code> and <code>-</code> (hyphen).</p></li>
<li><p>MUST NOT include spaces.</p></li>
<li><p>Multiple words in a segment MUST use <code>-</code> as the word
separator.</p>
<ul>
<li><p>Example: <code>storage.firmware-operator</code></p></li>
</ul></li>
</ul></td>
<td style="vertical-align: top;"><ul>
<li><p>The role name is used in GRN syntax to <a
href="https://developer.greenlake.hpe.com/docs/greenlake/services/iam/authorization/authz-v2/internal/openapi/authz-v2alpha1-api/operation/createRoleAssignment/">reference</a>
the role.</p></li>
<li><p>See <a
href="https://developer.greenlake.hpe.com/docs/greenlake/services/iam/authorization/authz-v2/internal/openapi/authz-v2alpha1-api/operation/createRole/">AuthZ
v2 Create Role API</a>.</p></li>
</ul></td>
</tr>
<tr>
<td style="vertical-align: top;"><p>Display Name</p></td>
<td style="vertical-align: top;"><p>Human-readable role name.</p></td>
<td style="vertical-align: top;"><ul>
<li><p>MUST use only <code>a-zA-Z0-9</code> <code>-</code> (hyphen) and
space.</p></li>
<li><p>MUST correspond to the role name.</p></li>
<li><p>MUST begin with the service name.</p></li>
<li><p>SHOULD use sentence case rather than title case.</p></li>
</ul></td>
<td style="vertical-align: top;"><ul>
<li><p>Used by the GLP UI when displaying the role name.</p></li>
</ul></td>
</tr>
<tr>
<td style="vertical-align: top;">Description</td>
<td style="vertical-align: top;">Friendly, human-readable description of the role.</td>
<td style="vertical-align: top;"><ul>
<li><p>MUST be a complete sentence.</p></li>
<li><p>MUST use sentence case and punctuation.</p></li>
<li><p>MUST provide additional, useful information about the
role.</p></li>
</ul></td>
<td style="vertical-align: top;"></td>
</tr>
<tr>
<td style="vertical-align: top;">Display Name and Description</td>
<td style="vertical-align: top;"></td>
<td style="vertical-align: top;"><ul>
<li><p>Display Name and Description MUST NOT be redundant.</p>
<ul>
<li><p>Example: Role name
<code>Workspace administrator</code> and description
“Workspace administrator.” is redundant.</p></li>
</ul></li>
</ul></td>
<td style="vertical-align: top;"></td>
</tr>
<tr>
<td style="vertical-align: top;"><em>All attributes above</em></td>
<td style="vertical-align: top;"></td>
<td style="vertical-align: top;"><ul>
<li><p>MUST NOT include the word “role” (it is redundant).</p>
<ul>
<li><p>Exception: When a role includes privileges to act on role
objects, it is permitted (example:
<code>Role administrator</code>).</p></li>
</ul></li>
<li><p>SHOULD use the Standard Personas or Granular Role Name
Syntax.</p></li>
</ul></td>
<td style="vertical-align: top;"></td>
</tr>
</tbody>
</table>

### Use of Standard Personas for Role Names

Role names SHOULD use the following Standard Personas.

- `Administrator` - Full control.

- `Operator` - Modify resources
  or perform actions, but typically cannot create or delete resources.

- `Viewer` - Read only access.

Role names MUST NOT use synonyms to name the standard personas. For
example, do not create a `Reader` role granting read only access, use
`Viewer`.

Role display names that grant privileges to a subset of resources MUST follow the
convention `<Service> <resource> <persona>`, like "Storage firmware administrator". Resource MUST be singular
(`task` not `tasks`).

Role display names MUST begin with the approved customer-facing service
name. Example:

| **Service** | **Role** | **Role Name** | **Role Display Name** |
|----|----|----|----|
| Storage | Administrator | **storage.administrator** | **Storage administrator** |
| Storage | Firmware administrator | **storage.firmware-administrator** | **Storage firmware administrator** |
| Storage | Operator | **storage.operator** | **Storage operator** |
| Storage | Firmware operator | **storage.firmware-operator** | **Storage firmware operator** |

### Example Role Descriptions

See Role Attribute Requirements table.

| **Role Display Name** | **Description** | **Standards Evaluation** |
|----|----|----|
| Storage operator | Operators view and edit resources but cannot create or delete them. | ✅ Acceptable |
| Storage operator | This role grants the Storage Operator role for the Storage Service. | ❌ Not Acceptable |
| Network task viewer | Grants read only access to network tasks. | ✅ Acceptable |
| Network task viewer | View tasks. | ❌ Not Acceptable |
| Compute administrator | Grants full access to compute resources. | ✅ Acceptable |
| Compute administrator | Grants administrator role. | ❌ Not Acceptable |

**Original publication date (yyyy-mm-dd):** 2024-11-14

**Revised:** 2025-01-22

**What's Changed** 2025-01-22

- In the "Description" row of the Permission Attribute Requirements table, add an additional "MUST" entry to require an explanation of what the permission allows.
