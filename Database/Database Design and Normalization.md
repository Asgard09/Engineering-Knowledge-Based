# 1. Normal Forms
## 1.1. First Normal Forms
Rule: Every column must contain atomic (indivisible) value
```
-- ❌ Violates 1NF (multi-value column)
| order_id | products          |
| 1        | "apple, banana"   |

-- ✅ 1NF compliant
| order_id | product   |
| 1        | apple     |
| 1        | banana    |
```
## 1.2. Second Normal Forms
