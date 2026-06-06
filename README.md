# Omada SDN Controller — Docker Password Reset

**Target:** TP-Link Omada Software Controller v5.x+  
**Environment:** Unraid Docker (image: `mbentley/omada-controller`, bundled legacy MongoDB)  
**Database:** `omada`  
**Auth:** Apache Shiro SHA-256 (password) + Base64-encrypted (username)

---

## The Problem

Standard password reset docs (targeting `db.user` with SHA-1) **fail on modern versions**. v5.x+ splits credentials across `db.iam_user` using Shiro SHA-256 hashing. The username field is also stored encrypted.

---

## Step 1 — Shell Into the Container

```bash
docker exec -it omada-controller bash
```

Then install required tools inside the container:

```bash
apt update && apt install -y curl gnupg
```

---

## Step 2 — Get a Legacy MongoDB Shell

Modern `mongosh` v2.x+ enforces a strict wire protocol that **rejects** Omada's bundled MongoDB engine. Download the standalone legacy binary inside the container:

```bash
curl -fSLO https://downloads.mongodb.com/compass/mongosh-1.10.6-linux-x64.tgz
tar -xvf mongosh-1.10.6-linux-x64.tgz
./mongosh-1.10.6-linux-x64/bin/mongosh --port 27217
```

---

## Step 3 — Switch to the Omada Database

At the `test>` prompt:

```js
use omada
```

---

## Step 4 — Find Your Account ObjectIds

```js
db.iam_user.find().pretty()
```

> ⚠️ ObjectIds are unique per install. Record your own `_id` values from this output before running the update commands below.

---

## Step 5 — Inject Recovery Credentials

The hash below resolves to the plaintext password: **`password`**

```js
// Replace YOUR_IAM_USER_ID and YOUR_USER_ID with the _id values from Step 4

db.iam_user.updateOne(
  { _id: ObjectId("YOUR_IAM_USER_ID") },
  { $set: {
    username: "HdJ2wNfCE8Aoy9UXQDy0MQ==",
    password: "$shiro1$SHA-256$500000$$Z85mqKxm1Lt0NJRw9jUlw3AzDQxrMHQWebk1kNb4pSM="
  }}
)

db.user.updateOne(
  { _id: ObjectId("YOUR_USER_ID") },
  { $set: { name: "admin" } }
)
```

---

## Step 6 — Verify

```js
db.iam_user.find({ _id: ObjectId("YOUR_IAM_USER_ID") }).pretty()
```

Confirm the `password` field exactly matches the `$shiro1$...` string above. No restart needed — changes are read immediately.

---

## Recovery Login

| Field    | Value      |
|----------|------------|
| Username | `admin`    |
| Password | `password` |

---

## ⚠️ Critical Security Step

After logging in, **immediately** go to **Settings > Account** and set a strong, unique password. This forces the controller to replace the public recovery hash with a new cryptographic signature unique to your instance.

---

## Hash Reference

| Purpose | Value |
|---------|-------|
| Shiro SHA-256 hash of `password` | `$shiro1$SHA-256$500000$$Z85mqKxm1Lt0NJRw9jUlw3AzDQxrMHQWebk1kNb4pSM=` |
| Encrypted Base64 for `admin` username | `HdJ2wNfCE8Aoy9UXQDy0MQ==` |
