---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# 🌈 Relationships

There are two ways for modules to have relationships.

1. A Direct Relationship via their entities.
2. An Indirect Relationship by having two(or more) entities being linked to the same token.

{% hint style="info" %}
Direct Relationships are easy to understand and usually suggested for clarity and scalability.

However, this is not true in MegaYours, as the token is used as a universal ID across different chains and modules. You should use the direct method only when really necessary.

Refrain from using direct relationships and instead use Indirect Relationships via the underlying token as much as possible to preserve the flexibility of the token and your module's interoperability and reusability.
{% endhint %}

In this page we will review the two different approaches and explain why an Indirect Relationship is the preferred approach.

## Direct Relationship

In a direct relationship a module refers to another one directly via an entity. Let's look at an example.

<pre class="language-kotlin"><code class="lang-kotlin"><strong>// students/model.rell
</strong><strong>entity student {
</strong><strong>  key yours.token;
</strong><strong>  name;
</strong><strong>  email: text;
</strong><strong>}
</strong><strong>
</strong><strong>// student_id_cards/model.rell
</strong>entity card {
  key students.student;
  issue_date: text;
}
</code></pre>

The benefits of modelling it like this is:

1. **Clarity and Simplicity:** The relationship between student and card is explicit and clear. It's easy to understand that each card is directly linked to a student. Queries that join student and card entities may be more simple to write because the relationship is direct.
2. **Data Integrity:** Ensures strong data integrity as the card entity directly references the student entity. This prevents orphaned records and ensures that every card is associated with a valid student.

## Indirect Relationship

Another approach that we can take is to use an Indirect Relationship. Let's look at the same two modules but with a indirect relationship instead.

```kotlin
// students/model.rell
entity student {
  key yours.token;
  name;
  email: text;
}

// student_id_cards/model.rell
entity card {
  key yours.token;
  issue_date: text;
}
```

Here both modules are not aware of each other, but they refer to the same underlying token.&#x20;

The benefits of modelling it like this are:

1. **Loose Coupling:** The entities are loosely coupled, making the system more flexible. Changes in one entity do not directly impact the other.
2. **Modularity:** Each entity can be managed independently, which is beneficial for large systems. This can simplify maintenance and scalability.
3. **Reusability:** The yours.token can be reused across different modules, making it easier to link various entities without direct dependencies.

## Summary

We highly recommend using the Indirect Relationship approach. The key features of Yours Protocol are flexibility, modularity, and reusability. It's important for modules to be shareable, which is much easier if each module is self-contained and doesn't directly depend on other modules.

A Direct Relationship may simplify querying slightly in certain scenarios, but we only recommend using it in modules that are specific to your dApp and ones that you do not intend to share or promote as reusable.
