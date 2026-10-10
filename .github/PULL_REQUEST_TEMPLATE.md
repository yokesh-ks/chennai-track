---
name: Pull Request
about: Template for Chennai Track pull requests
title: "[PR] <short title>"
labels: ["Process"]
assignees: ""
---

# Pull Request

## Summary

<Describe what this PR changes and why. Keep it to a few sentences.>

## Linked Work

- Closes #<issue-number>
- Related to #<issue-number>

---

## Checklist

### Process
- [ ] Issue this PR against the `develop` branch
- [ ] Linked issue(s) referenced above
- [ ] Agent attribution recorded (agent name + task ID)

### Code Quality
- [ ] Code follows existing style and conventions
- [ ] No hardcoded secrets, keys, or credentials
- [ ] No debug logging left in production paths
- [ ] New behavior is covered or the change is infrastructure-only

### Verification
- [ ] `npm run lint` passes
- [ ] `npm run typecheck` passes
- [ ] `npm test` passes
- [ ] Manual verification performed

### Product
- [ ] Operational claims expose source and freshness (scheduled vs live)
- [ ] Accessibility baseline maintained (WCAG 2.2 AA)
- [ ] Privacy requirements respected (no personal data persisted without consent)

---

## Notes

- <Anything reviewers should pay special attention to.>