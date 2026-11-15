# SQL Queries for SaaS Billing & Support Triage

Sample PostgreSQL queries used for investigating failed payments, unsettled invoices, and account usage anomalies.

### 1. Identify Unsettled Invoices Past Due Date
```sql
SELECT 
    c.customer_id,
    c.email,
    i.invoice_id,
    i.amount_due,
    i.due_date,
    i.status
FROM invoices i
JOIN customers c ON i.customer_id = c.customer_id
WHERE i.status IN ('unsettled', 'failed')
  AND i.due_date < CURRENT_DATE
ORDER BY i.due_date ASC;
```

### 2. Audit Failed Payment Attempts by Error Code
```sql
SELECT 
    organization_id,
    payment_intent_id,
    decline_code,
    attempt_count,
    created_at
FROM payment_logs
WHERE status = 'failed'
  AND created_at >= NOW() - INTERVAL '7 days'
ORDER BY created_at DESC;
```
