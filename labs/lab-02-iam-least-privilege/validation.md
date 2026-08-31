# Validation

## Test User

developer1

---

## Expected

| Test | Expected |
|-------|----------|
| Open IAM | Denied |
| Open S3 | Allowed |
| Create Bucket | Denied |

---

## Actual

| Test | Result |
|-------|--------|
| Open IAM | PASS |
| Open S3 | PASS |
| Create Bucket | PASS |

---

## Conclusion

The custom IAM policy successfully enforced least privilege.
