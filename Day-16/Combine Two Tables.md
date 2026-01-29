# 175. Combine Two Tables

## Problem Brief
You are given two tables:
- `Person(personId, firstName, lastName)`
- `Address(addressId, personId, city, state)`

Return the first name, last name, city, and state of **every person**.  
If a person does not have an address, return `NULL` for city and state.

---

## Approach
- Use a **LEFT JOIN**
- `Person` is the left table to ensure all people are included
- Join condition: `Person.personId = Address.personId`
- Missing address rows automatically produce `NULL` values

---

## SQL (MySQL)

```sql
SELECT 
    p.firstName,
    p.lastName,
    a.city,
    a.state
FROM Person p
LEFT JOIN Address a
ON p.personId = a.personId;
