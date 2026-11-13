# Changelog

All notable changes to `qalainau/filament-warp-input` are documented here.

## [1.1.0] - 2026-11-13

### Added

- **`PickList`:** choose many items from two lists. Double-click, the arrow buttons or drag to move items; search in both lists; put the chosen items in order with `reorderable()` (saved in the array, or in a pivot column with `reorderable(pivotColumn: 'sort')`); `maxItems()`; with `relationship()`, many records are searched on the server (`serverSearchAbove()`). Saves the same as `Select` with `multiple()`.
- **`RecordSelect::columns([...])`:** the list as a table with a heading over each column, so similar records can be told apart by code, price or stock. `OptionColumn` formats money like `MoneyColumn`, numbers, or your own text. The saved key doesn't change.
- **`MentionTextarea` and `MentionEntry`:** `@` suggests people and `#` suggests records while typing. The text is saved in your own column with the keys (`@[Aiko Tanaka](12)`), so older notes read as before; `onMention()` gets the people newly mentioned once the save is final. Mentions are checked on the server and limited to the current team.
- **Paste to fill (`FormAssist::make()->smartPaste()`):** paste an order email or a business card and Claude fills in the form's fields and the rows of a repeater, through the same conversions as typing (choices by label, phone numbers and amounts formatted by their fields). Filled fields are marked **AI** until changed. The pasted text is sent to Anthropic when the user presses the button; without a key the button isn't shown. Use `using:` for another service.
- **`TextEntry::warpEditable()`:** edit a value right on the view page with a Warp Input field. Enter saves, Esc cancels; only people who may update the record see the pencil, and a save stops when someone else saved first.

None of these change existing fields or need a new table.

## [1.0.1] - 2026-11-12

### Changed

- Now requires Filament 5.11 or later (was 5.0). Parts of the plugin did not work on older 5.x versions, which were never tested. The tests now also run on the lowest supported versions before each release.

## [1.0.0] - 2026-11-11

### Added

- **Phone numbers:** `PhoneInput` (a country list, formatting while typing, pasted and full-width numbers, extensions, mobile-only fields, E.164 storage), `PhoneColumn`, `PhoneEntry` and the `Phone` rule.
- **Email and web addresses:** `EmailInput` (one-click typo fixes, disposable addresses, tidy domains), `UrlInput` (adds `https://`, strips tracking parameters, limits domains, social profiles from `@handle`), `UrlColumn`, `UrlEntry`, and the `NotDisposableEmail`, `UrlDomain` and `SocialProfile` rules.
- **Duplicate warnings:** `->warnDuplicates()` on phone, email, web address and text fields — "Another customer already has this email address", per team.
- **Addresses:** `AddressFields` (fields that follow the country for about 250 countries and regions, saved into your columns or one JSON column), postal code lookup from files imported with `php artisan warp-input:postal-codes` (Japan Post and GeoNames), `AddressMap` (Leaflet and OpenStreetMap, no API key; clicking the map fills in the address of that place), filling one address column from the postal code (`PostalCodeInput::fillAddress()`), optional address suggestions and geocoding, `CountrySelect`, `RegionSelect`, `TimezoneSelect`, `CurrencySelect`, `LanguageSelect`, `AddressColumn`, `AddressEntry`, `CountryColumn`, `CountryEntry`, and the `AddressPostalCode` and `AddressRegion` rules.
- **Money and numbers:** `MoneyInput` (formatting for each locale and currency, short forms like *1.5k* and *3万*, tax included or excluded, rounding, exchange-rate estimates), `NumberInput` (− / + buttons, units, pack sizes), `DiscountInput` (% or amount) — drawn as `type="tel"` so a Japanese, Chinese or Korean input method can't type full-width digits, `RatingInput`, `->calculate()` formulas recalculated on the server, `MoneyColumn`, `MoneyEntry`, `RatingColumn`, `RatingEntry`, the `MoneySum`, `TaxSummary` and `RatingAverage` summarizers, and the `Money` rule.
- **Choosing:** `RecordSelect` (search by initials, code or reading in kana and romaji, add records inline, recently used first) and `SuggestInput`.
- **Text:** `WarpTextInput` and `WarpTextarea` (character counters that count emoji as one, full-width as two, bytes or text messages; normalizing half-width, full-width and kana; furigana from the Japanese IME; saved replies; merge tags; word and personal data warnings), `PasswordInput` (strength, rules ticked off as you type, generate and copy), `MergeTags::render()`, and the `MaxCharacters`, `MaxBytes`, `MaxSmsMessages`, `RejectWords`, `KnownMergeTags` and `Kana` rules.
- **IDs:** `IdInput` (IBAN, EU, UK and Swiss VAT numbers with an optional VIES check, Japanese invoice, corporate and My Number numbers, GTIN, EAN, UPC, ISBN, postal codes for 56 countries, card numbers that are never saved), `SequenceInput` and `Sequence::next()` (numbers in order, never twice), `->masked()`, `IdColumn`, `IdEntry`, `MaskedColumn`, `MaskedEntry` and the `ValidId` rule.
- **Capture:** `SignaturePad`, `StampInput` (name and date stamps), `ScanInput` and `->scan()` (barcodes and QR codes with the camera), `->scanText()` (text from a photo), `->dictation()`, `SignatureColumn`, `SignatureEntry`, `StampColumn`, `StampEntry`, `Stamp::svg()` and the `Barcode` rule.
- **Whole forms:** `FormAssist` with drafts that survive a closed tab, conflict detection when someone else saved first, Enter to the next field, a summary of required fields still empty, copy from the previous record, undo and redo, and paste from a spreadsheet.
- Panel defaults on `FilamentWarpInputPlugin` (country, preferred countries, currency, tax rate, rounding, lookups), Filament tenancy, ten languages, and Laravel Boost guidelines and a skill.
