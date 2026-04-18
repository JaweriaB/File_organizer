---
title: "SQL Injection for Absolute Beginners: How I Cracked My First Lab in 24 Hours"
datePublished: 2026-04-18T14:13:35.775Z
cuid: cmo4f3noc002p2cmi66oce4r0
slug: sql-injection-for-absolute-beginners-how-i-cracked-my-first-lab-in-24-hours
cover: https://cdn.hashnode.com/uploads/covers/69e0e67094b913c91338c0de/3cd0f096-13ce-42af-9797-fe1e36b6d058.png

---

*By Jaweria Batool | Cybersecurity Learner | PortSwigger Web Academy*

* * *

## Wait — Someone Can Hack a Website With a Single Quote Mark?

Yeah. One character. `'`

That's it. That's the villain of this story.

I'm a self-taught cybersecurity student. No fancy lab setup, no university professor walking me through this. Just me, a browser, and PortSwigger Web Academy wondering why a single punctuation mark can bring an entire login system to its knees.

So if you've ever heard the term "SQL Injection" and thought *"sounds complicated, not for me"* — sit down. I promise you'll get it by the end of this. Because if I got it, you definitely will.

* * *

## Okay So Basically — Imagine This...

You walk into a pizza place. You order a **Margherita**. The waiter goes to the kitchen, checks the system, finds your order, brings it back. Beautiful. Civilized. Normal.

Now imagine you say:

> *"One Margherita —* ***or actually, just bring everything in the kitchen.****"*

And the chef just... does it. No questions asked. Every pizza. Every dish. Every secret recipe they were saving for VIP customers. 🍕💀

You didn't break in. You didn't pick a lock. You just said something the system wasn't prepared to hear — and it obeyed.

**That's SQL Injection.**

You're not hacking in the Hollywood sense. You're just talking to the database in a way the developer never expected. And the database, no questions asked, just listens.

* * *

## The Technical Part (Don't Run — I'll Make It Make Sense)

Every website that has a login, a search bar, or a product page is talking to a **database** (think: a giant organized spreadsheet living on a server) using something called **SQL** (Structured Query Language — basically the language databases speak).

When you search for "Gaming Laptops" on a laptop shop, behind the scenes the website sends something like this to the database:

```sql
SELECT * FROM products WHERE category = 'Gaming Laptops' AND released = 1
```

What that actually means:

> *"Show me everything from the products table where the category is Gaming Laptops AND the product is already released."*

Normal. Clean. Fine.

But what if instead of typing `Gaming Laptops` you typed:

```plaintext
Gaming Laptops'--
```

Now the query becomes:

```sql
SELECT * FROM products WHERE category = 'Gaming Laptops'--' AND released = 1
```

See that `--`? In SQL that means **"ignore everything after this."** It's a comment marker.

So the database now reads:

> *"Show me everything where category is Gaming Laptops"* — and completely ignores the `AND released = 1` part.

**Result?** Every laptop shows up. Including the unreleased ones the shop wasn't ready to sell yet. The database just handed you the secret menu. 😅

* * *

## The Login Bypass — This One Will Make Your Jaw Drop

Here's where it gets genuinely unhinged.

Normal login query:

```sql
SELECT * FROM users WHERE username = 'admin' AND password = 'mypassword'
```

Now type this as the username: `administrator'--`

Query becomes:

```sql
SELECT * FROM users WHERE username = 'administrator'--' AND password = ''
```

The password check? **Gone.** Commented out. You just logged in as administrator with no password.

One quote mark. That's literally all it took.

> Okay, find the user named administrator... oh wait, there's a comment marker. I'll just ignore the rest of this command!

Result: Welcome, Administrator. No password needed. 👀

No brute force. No fancy tools. Just punctuation doing crimes.

* * *

## So How Do You Actually Stop This?

The fix is something called **Parameterized Queries** (also called Prepared Statements).

Instead of building the query by gluing user input directly into it like:

```sql
"SELECT * FROM products WHERE category = '" + userInput + "'"
```

You do this:

```sql
SELECT * FROM products WHERE category = ?
-- then pass userInput separately
```

Now the database treats user input as **pure data**, never as part of the actual command. You could type `Gaming Laptops'--` all day and the database would just search for a product literally named that. No injection. No damage.

Simple fix. Massive difference. Developers have been forgetting it since 1998.

* * *

## Okay But Does This Actually Work? I Tried It. Here's What Happened.

Theory is great. But I needed to see it with my own eyes.

So I opened **PortSwigger Web Academy Lab 1** — *"SQL injection vulnerability in WHERE clause allowing retrieval of hidden data."*

The lab gives you a fake shopping site. 12 products total. You can filter by category. Looks completely normal.

Here's what the URL looks like when you click a category:

```plaintext
https://[lab-url]/filter?category=Accessories
```

And behind the scenes the database is running:

```sql
SELECT * FROM products WHERE category = 'Accessories' AND released = 1
```

That `released = 1` part? That's the gate. It's hiding unreleased products from you.

So I typed this into the URL instead:

```plaintext
/filter?category=Accessories'+OR+1=1--
```

Query becomes:

```sql
SELECT * FROM products WHERE category = 'Accessories' OR 1=1--' AND released = 1
```

What that actually means:

> *"Show me everything where category is Accessories OR where 1 equals 1."*

And since 1 always equals 1 — **every single product showed up.** Released. Unreleased. All of it. 😶

The lab marked as solved immediately.

I stared at the screen for a second like — that's it? That's the hack?

Yeah. That's the hack. One line. No tools. No special software. Just a URL.

* * *

**The flow if you're visual:**

```plaintext
Normal request:
URL → category=Accessories → Database checks: category AND released=1 → Shows 3 products

Injected request:
URL → category=Accessories' OR 1=1-- → Database checks: category OR always-true → Shows ALL products
```

* * *

## The Takeaway

SQL Injection isn't some obscure theoretical attack. It's been behind some of the biggest data breaches in history — millions of passwords, credit card numbers, personal records. All because someone forgot to separate user input from database commands.

Here's what I want you to remember:

> **Databases are literal. They do exactly what they're told. SQL Injection is just telling them something the developer never intended.**

I learned this yesterday. Solved my first two labs on PortSwigger. And now you understand it too.

Next up I'm going into UNION attacks — where things get even more interesting (and slightly more chaotic 😄). Follow along so you don't miss it.

**Try It Yourself:** Head to [portswigger.net/web-security/sql-injection](https://portswigger.net/web-security/sql-injection) — the first two Apprentice labs are free. See if you can break a login with exactly what you just learned.

*Drop a comment if something didn't click — I'm learning this alongside you, so zero judgment here.* 👇

* * *

*I'm Jaweria. Still figuring out cybersecurity, one vulnerability at a time. If you're also learning from scratch — you're in the right place.*