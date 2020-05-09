# Migration `20200926182140-remove-model-mapping`

This migration has been generated by Tom Dohnal at 9/26/2020, 8:21:40 PM.
You can check out the [state of the schema](./schema.prisma) after the migration.

## Database Steps

```sql
ALTER TABLE "public"."TweetHashtag" DROP CONSTRAINT "TweetHashtag_tweetId_fkey"

ALTER TABLE "public"."TweetMedia" DROP CONSTRAINT "TweetMedia_tweetId_fkey"

ALTER TABLE "public"."TweetMediaSize" DROP CONSTRAINT "TweetMediaSize_tweetMediaId_fkey"

ALTER TABLE "public"."TweetSymbol" DROP CONSTRAINT "TweetSymbol_tweetId_fkey"

ALTER TABLE "public"."TweetUnwoundUrl" DROP CONSTRAINT "TweetUnwoundUrl_tweetId_fkey"

ALTER TABLE "public"."TweetUnwoundUrl" DROP CONSTRAINT "TweetUnwoundUrl_tweetUrlId_fkey"

ALTER TABLE "public"."TweetUrl" DROP CONSTRAINT "TweetUrl_tweetId_fkey"

ALTER TABLE "public"."TweetUserMention" DROP CONSTRAINT "TweetUserMention_tweetId_fkey"

DROP TABLE "public"."TweetHashtag"

DROP TABLE "public"."TweetMedia"

DROP TABLE "public"."TweetMediaSize"

DROP TABLE "public"."TweetSymbol"

DROP TABLE "public"."TweetUnwoundUrl"

DROP TABLE "public"."TweetUrl"

DROP TABLE "public"."TweetUserMention"
```

## Changes

```diff
diff --git schema.prisma schema.prisma
migration 20200912191941-fix-relationship..20200926182140-remove-model-mapping
--- datamodel.dml
+++ datamodel.dml
@@ -2,9 +2,9 @@
 // learn more about it in the docs: https://pris.ly/d/prisma-schema
 datasource db {
   provider = "postgresql"
-  url = "***"
+  url = "***"
 }
 generator client {
   provider        = "prisma-client-js"
@@ -42,114 +42,23 @@
   updatedAt DateTime @updatedAt
   tweets    Tweet[]  @relation(references: [id])
 }
-model TweetHashtag {
-  id        Int      @id @default(autoincrement())
-  text      String
-  indices   Int[]
-  tweet     Tweet    @relation(fields: [tweetId], references: [id])
-  tweetId   Int
-  createdAt DateTime @default(now())
-  updatedAt DateTime @updatedAt
-}
-
-model TweetMediaSize {
-  id           Int        @id @default(autoincrement())
-  width        Int
-  height       Int
-  resize       String
-  tweetMedia   TweetMedia @relation(fields: [tweetMediaId], references: [id])
-  tweetMediaId Int
-  createdAt    DateTime   @default(now())
-  updatedAt    DateTime   @updatedAt
-}
-
-model TweetMedia {
-  id            Int              @id @default(autoincrement())
-  displayUrl    String
-  expandedUrl   String
-  indices       Int[]
-  mediaUrl      String
-  mediaUrlHttps String
-  sizes         TweetMediaSize[]
-  type          String
-  url           String
-  tweet         Tweet            @relation(fields: [tweetId], references: [id])
-  tweetId       Int
-  createdAt     DateTime         @default(now())
-  updatedAt     DateTime         @updatedAt
-}
-
-model TweetUnwoundUrl {
-  id          Int      @id @default(autoincrement())
-  url         String
-  status      Int
-  title       String
-  description String
-  tweetUrl    TweetUrl @relation(fields: [tweetUrlId], references: [id])
-  tweetUrlId  Int
-  tweet       Tweet    @relation(fields: [tweetId], references: [id])
-  tweetId     Int
-  createdAt   DateTime @default(now())
-  updatedAt   DateTime @updatedAt
-}
-
-model TweetUrl {
-  id          Int              @id @default(autoincrement())
-  displayUrl  String
-  expandedUrl String
-  indices     Int[]
-  url         String
-  unwound     TweetUnwoundUrl?
-  tweet       Tweet            @relation(fields: [tweetId], references: [id])
-  tweetId     Int
-  createdAt   DateTime         @default(now())
-  updatedAt   DateTime         @updatedAt
-}
-
-model TweetUserMention {
-  id         Int      @id @default(autoincrement())
-  indices    Int[]
-  name       String
-  screenName String
-  tweet      Tweet    @relation(fields: [tweetId], references: [id])
-  tweetId    Int
-  createdAt  DateTime @default(now())
-  updatedAt  DateTime @updatedAt
-}
-
-model TweetSymbol {
-  id        Int      @id @default(autoincrement())
-  indices   Int[]
-  text      String
-  tweet     Tweet    @relation(fields: [tweetId], references: [id])
-  tweetId   Int
-  createdAt DateTime @default(now())
-  updatedAt DateTime @updatedAt
-}
-
 model Tweet {
-  id                     Int                @id @default(autoincrement())
-  twitterId              String             @unique
+  id                     Int         @id @default(autoincrement())
+  twitterId              String      @unique
   publishedAt            DateTime
   text                   String
   accountName            String
   accountScreenName      String
   accountProfileImageUrl String
-  account                Account            @relation(fields: [accountId], references: [id])
+  account                Account     @relation(fields: [accountId], references: [id])
   accountId              Int
   favoritesCount         Int
   retweetsCount          Int
-  hashtags               TweetHashtag[]
-  media                  TweetMedia[]
-  urls                   TweetUrl[]
-  userMentions           TweetUserMention[]
-  symbols                TweetSymbol[]
-  tweetTypes             TweetType[]        @relation(references: [id])
-  createdAt              DateTime           @default(now())
-  updatedAt              DateTime           @updatedAt
-  TweetUnwoundUrl        TweetUnwoundUrl[]
+  tweetTypes             TweetType[] @relation(references: [id])
+  createdAt              DateTime    @default(now())
+  updatedAt              DateTime    @updatedAt
 }
 enum Period {
   DAY
```

