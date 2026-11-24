# CodeRabbit severity map

Map CodeRabbit CLI severities to `$code-review` provisional severities before
returning candidates:

| CodeRabbit | provisional_severity |
| ---------- | -------------------- |
| critical   | Critical             |
| major      | High                 |
| minor      | Medium               |
| trivial    | Low                  |
| info       | Low                  |
| none       | drop (do not return) |

After mapping, `$code-review` may still re-evaluate severity during normalize
and verify. Do not promote severity solely because CodeRabbit labeled it
critical; verification owns the final finding severity.
