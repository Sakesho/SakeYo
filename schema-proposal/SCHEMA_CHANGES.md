# Schema Changes — Kinfolk-NS Prototype Support

Proposed updates to `prisma/schema.prisma` to back the Kinfolk-NS HTML prototype
(`/Users/bradbrown/Desktop/SakeYo/Kinfolk-NS/`).

- **Source of truth:** `schema.prisma` (drop-in replacement — diff against the
  existing `sakesho-backend-main/prisma/schema.prisma`)
- **ERD:** paste `schema.dbml` into [dbdiagram.io](https://dbdiagram.io) →
  interactive diagram with 8 table groups and FK arrows
- **Nothing in the existing repo is modified.** All three files live in this
  folder only.

---

## How to diff against the current schema

```bash
diff "/Users/bradbrown/Desktop/Sakesho Repo/sakesho-backend-main/prisma/schema.prisma" \
     "/Users/bradbrown/Desktop/SakeYo/schema-proposal/schema.prisma"
```

Every addition is flagged with `// [KINFOLK]` in the Prisma file.
Every new field on an existing model carries the same tag inline.

---

## Summary

- **17 new models** (11 primary + 6 join/child tables)
- **3 new relation fields** on `SakeBottle`, `LearnArticle`, `UserSakeDiaryEntry`
- **4 new fields** on `UserSakeDiaryEntry` (`pouredTemp`, `vesselShape`, `styleFlags`, `pairings` relation)
- **3 new fields** on `SakeBottle` (drinking-window hints for Cellar)
- **11 new enums**
- **Zero destructive changes** — every existing field is preserved verbatim.

---

## Gap analysis — which prototype page drove each addition

| Prototype page(s) | New/updated models | Why |
|---|---|---|
| `sakeyo_Cellar.html`, `Cellar_Add.html`, `Cellar_Detail.html` | `CellarEntry` (new); `SakeBottle.drinkWindowMonthsAfterBrewing/AfterOpening/lightSensitive` (new fields) | Physical inventory is distinct from ratings/diary. Drink-window hints on the bottle let the UI compute per-entry timelines without per-user calibration. |
| `sakeyo_Wishlist.html` | `Place`, `UserSavedPlace` (new) | Wishlist shows curated *places* (bars, restaurants, tasting rooms, shops, brewery rooms), not bottles. Distinct from `SakeBrewery` (production entity). |
| `sakeyo_Passport.html` | `PassportEntry`, `PassportEntryBottle`, `PassportPhoto` (new) | "Places you've been" with per-visit photos and what-we-drank rows. Photos normalized out to allow ordered multi-image uploads. |
| `sakeyo_Calendar.html`, `Calendar_Agenda.html`, `Event_Detail.html` | `Event`, `UserSavedEvent`, `EventReservation` (new) | 6 event types, host metadata, seat-limit tracking for "X seats left" UI, RSVP flow. |
| `sakeyo_Alerts.html` | `Alert` (new) | Polymorphic inbox — `entityType` + `entityId` refs any of CellarEntry, Event, CounterPost, LearnArticle, SakeBottle. `deepLinkPath` lets the client router navigate without hardcoding entity→URL logic. |
| `sakeyo_Counter.html`, `Counter_Note.html` | `CounterPost`, `CounterReply`, `UserSavedCounterPost` (new) | Single `CounterPost` table with `type` enum (SAKE_SHARE / ASK / TRIP) gives a unified feed — type-specific columns (`tripCity`, `askContextImageUrl`) are nullable. Endorsed replies flagged on `CounterReply.isEndorsed`. |
| `sakeyo_Diary_Add.html`, `Diary_Detail.html` | `DiaryPairing` (new); `UserSakeDiaryEntry.pouredTemp/vesselShape/styleFlags/pairings` (new) | Existing diary model had free-text `servingTemp` and `location` but no vessel, style multi-select, or multi-row pairings. `servingTemp` kept for back-compat; new `pouredTemp` enum is canonical. |
| `sakeyo_Pairings.html` | `UserSavedLearnArticle` (new) | Editorial pairing guides are `LearnArticle` rows with `category = "pairing_guide"`. Bookmark button = `UserSavedLearnArticle` row. |
| `sakeyo_Profile.html` | `UserProfile` (new) | Display-facing fields (avatar, displayName, joinedAt, bio, public-profile slug, cached signature quote) — deliberately separate from `UserTasteProfile` which is the taste-math table. |
| `sakeyo_Account_Setup.html` | `UserOnboardingState` (new) | Tracks step completion for onboarding nudges. |
| `sakeyo_Pairing.html`, `Pairing_Result.html` | none | Pairing engine is stateless — consumes `UserTasteProfile` + `SakeProfileInfo` + `SakeBottle`. No persistence needed unless you want to log query history (future work). |
| `sakeyo_Scan_*.html` | none | Existing `ImageAnalysis` + `UnrecognizedSake` already cover the scan flow. |
| `sakeyo_Quiz.html`, `Quiz_Retake.html`, `Palate_Tune.html` | none | Existing `QuizResults` + `UserTasteProfile` cover these. Palate Tune writes directly to `UserTasteProfile` weights. |
| Everything else (Home, Bottle_Detail, Breweries, Search, Explore, 101_*, Article, Japan, Local, Craft, Sign_In, Settings) | none (consumes existing) | |

---

## New models — master-doc-style reference

The existing Excel master doc has one sheet per model, first row = field names.
Below is the equivalent header spec for every new table, ready to paste as new sheets.

### cellar_entries
| id | userId | sakeBottleId | purchaseDate | source | price | priceCurrency | volumeMl | status | openedAt | finishedAt | storageType | location | notes | drinkByAlertsEnabled | drinkByDate | createdAt | updatedAt |

### diary_pairings
| id | diaryEntryId | dishName | reactionNote | orderIndex | createdAt |

### places
| id | name | type | prefecture | city | address | website | phone | description | imageUrl | sourceQuote | sourceLearnArticleId | latitude | longitude | createdAt | updatedAt |

### user_saved_places
| id | userId | placeId | savedAt |

### passport_entries
| id | userId | placeId | visitDate | durationHours | notes | sourceLearnArticleId | createdAt | updatedAt |

### passport_entry_bottles
| id | passportEntryId | sakeBottleId | bottleName | rating | notes | createdAt |

### passport_photos
| id | passportEntryId | imageUrl | caption | orderIndex | createdAt |

### events
| id | slug | name | type | lede | description | imageUrl | startAt | endAt | durationMinutes | timezone | placeId | venueName | venueAddress | prefecture | price | priceCurrency | maxSeats | reservedSeats | hostUserId | hostName | hostRole | hostBio | hostAvatarUrl | marketplaceTieInText | isFeatured | published | publishedAt | createdAt | updatedAt |

### user_saved_events
| id | userId | eventId | savedAt |

### event_reservations
| id | userId | eventId | seats | status | createdAt | updatedAt |

### alerts
| id | userId | type | entityType | entityId | title | description | isRead | readAt | deepLinkPath | createdAt |

### counter_posts
| id | userId | type | headline | body | sakeBottleId | tripCity | tripPrefecture | askContextImageUrl | isPinned | createdAt | updatedAt |

### counter_replies
| id | counterPostId | userId | body | isEndorsed | createdAt | updatedAt |

### user_saved_counter_posts
| id | userId | counterPostId | savedAt |

### user_saved_learn_articles
| id | userId | learnArticleId | savedAt |

### user_profiles
| id | userId | displayName | avatarUrl | bio | isProfilePublic | publicProfileSlug | signatureQuote | signatureQuoteGeneratedAt | joinedAt | createdAt | updatedAt |

### user_onboarding_states
| id | userId | hasCompletedQuiz | hasSetDisplayName | hasScannedFirstLabel | hasLoggedFirstDiaryEntry | hasAddedCellarBottle | onboardingCompletedAt | createdAt | updatedAt |

---

## Modified existing models

### user_sake_diary_entries — additions
| Field | Type | Notes |
|---|---|---|
| `pouredTemp` | `PouredTemp?` | Enum: COLD/ROOM/WARM/HOT. Canonical replacement for free-text `servingTemp` (legacy kept). |
| `vesselShape` | `VesselShape?` | Enum: HIRAGATA/SUGI_NARI/TSUTSU_GATA/WINE_GLASS/OTHER. |
| `styleFlags` | `String[]` default `[]` | Multi-select from Diary_Add Style chips (Mellow/Heavy/Aged/Fruity/Dry/Light). |
| `pairings` (relation) | `DiaryPairing[]` | Backref from `DiaryPairing.diaryEntryId`. |

### sake_bottles — additions
| Field | Type | Notes |
|---|---|---|
| `drinkWindowMonthsAfterBrewing` | `Int?` | Recommended window from brew date. |
| `drinkWindowMonthsAfterOpening` | `Int?` | Recommended window once opened. |
| `lightSensitive` | `Boolean` default `false` | Surfaces "Sensitive to light" warning on Cellar_Detail. |
| `cellarEntries` (relation) | `CellarEntry[]` | |
| `counterPosts` (relation) | `CounterPost[]` | |
| `passportBottles` (relation) | `PassportEntryBottle[]` | |

### learn_articles — additions (relations only, no new columns)
| Relation | Points to |
|---|---|
| `savedByUsers` | `UserSavedLearnArticle[]` |
| `places` | `Place[]` (source attribution) |
| `passportEntries` | `PassportEntry[]` (source attribution) |

---

## Migration notes

1. **Zero breaking changes.** All new fields are nullable or have defaults.
   Existing reads/writes continue to work unmodified.

2. **`servingTemp` → `pouredTemp`.** Keep both during transition. Backfill job:
   ```
   pouredTemp = map(servingTemp.toLowerCase()) where known
   ```
   After client deploys, new writes populate `pouredTemp` only; a later
   migration can drop `servingTemp`.

3. **`CellarEntry.drinkByDate` is a cache.** Populate via (a) write-time trigger
   that reads `openedAt` + `sakeBottle.drinkWindowMonthsAfterOpening`, or
   (b) nightly job. The UI tolerates `null` (falls back to style defaults).

4. **`Event.reservedSeats` counter.** Increment/decrement in a transaction
   with the `EventReservation` upsert. Don't derive at read time on the hot
   path — calendar queries would scan too many reservations.

5. **RLS policies (Supabase).** Every table with a `userId` column should
   have a row-level-security policy restricting select/insert/update/delete
   to `auth.uid() = userId`. Tables this applies to:
   - `cellar_entries`, `user_saved_places`, `passport_entries`, `user_saved_events`,
     `event_reservations`, `alerts`, `user_saved_counter_posts`, `user_saved_learn_articles`,
     `user_profiles`, `user_onboarding_states`
   - `counter_posts` and `counter_replies` are **public-read, owner-write**
     (community feed). Different policy.

6. **Polymorphic `Alert.entityId`.** Not FK-enforced by design. Writer is
   responsible for setting `entityType` correctly. Consumer switches on
   `entityType` to resolve.

7. **`Place` vs `SakeBrewery`.** Intentionally separate — a brewery that
   accepts visitors gets *both* a `SakeBrewery` row (production entity) and a
   `Place` row (visitable venue). Soft-linked by name/address to keep the
   editorial catalogs independent. If this becomes painful, add an optional
   `Place.sakeBreweryId` FK later.

---

## Open questions for the developer / product

These came up during analysis and are NOT decided in the schema — flagged so
you can resolve before migration:

1. **Does Palate Tune persist anywhere beyond `UserTasteProfile`?** The prototype
   implies it writes directly to the profile's weights. If there's a desire
   to distinguish "quiz-derived" vs "manually tuned" weights, we'd need a
   `UserPalateCustomization` table. Not added yet — ask the ML/personalization
   team.

2. **Event hosts — normalize or not?** Currently denormalized (`hostName`,
   `hostBio`, `hostAvatarUrl` inline on `Event`). Normalize into an
   `EventHost` table if the same host recurs across events. Worth revisiting
   after seeing real editorial data.

3. **Counter post → reply notifications.** The `Alert` table supports
   `COMMUNITY_REPLY` but there's no built-in trigger to fan out alerts when
   a reply is posted. That's application-layer, not schema.

4. **Unified "saved" / bookmark table?** Currently four separate join tables
   (`UserSavedPlace`, `UserSavedEvent`, `UserSavedCounterPost`,
   `UserSavedLearnArticle`). Pro: Prisma-native FKs and Postgres integrity.
   Con: more tables. A polymorphic `UserBookmark(userId, entityType, entityId)`
   would be one table but loses FK safety. Recommendation: keep separate.

5. **Scan menu flow.** `sakeyo_Pairing_Result.html` CTAs "Find one on menu" —
   implies a future menu-scan feature. Would reuse `ImageAnalysis` but with
   a different prompt. No schema change needed now.

6. **Account_Setup fields.** The prototype page wasn't deeply inspected in
   the inventory (the agent noted "likely onboarding questions"). If it
   captures demographic / preference fields beyond the taste quiz (e.g.,
   home prefecture, frequency of drinking), those may need columns on
   `UserProfile`. Worth a 5-min look before migration.
