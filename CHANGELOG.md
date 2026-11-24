# Changelog

All notable changes to `qalainau/filament-warp-input` are documented here.

## [Unreleased]

## [1.2.2] - 2026-11-25

### Fixed

- **Text typed before a field's script finished loading was lost**: Warp Input fields load their script when they appear, and on a slow connection or a busy device the first characters typed (or a click on “Create” right away) went nowhere: the field was emptied when the script arrived, and the record could be saved without them. Those characters now reach the form and stay in the field. A re-render while the script was still loading (for example a validation error) also no longer shows JavaScript errors such as `noteLevel is not defined`. Nothing to change in your app; if you added a workaround that waits for `[data-warp-input-ready]` before typing, you can remove it.

## [1.2.1] - 2026-11-24

### Fixed

- **The “Stamp” button was hard to read in dark mode**: white text on the light primary color (contrast 1.86 with a teal panel). In dark mode the button text is now dark (contrast above 10:1); the same goes for “Use this text” when reading text from a photo. Light mode is unchanged.

## [1.2.0] - 2026-11-23

### Added

- **Address suggestions from Google or Mapbox**: `GooglePlaces` and `Mapbox` can be passed to `WarpInput::addressSuggestions()` if you have a key. They are called from your server, so the key never reaches the browser. The default suggestions are unchanged. See [Address suggestions while typing](README.md#address-suggestions-while-typing).
- **Rewrite with AI: `aiRewrite()`** on `WarpTextInput` and `WarpTextarea`: a button offers “Make polite”, “Shorten”, “Bullet points”, “Translate to English” or your own instructions, shows the original and the rewritten text side by side, and lets you replace (and undo) or cancel. Off unless you call it. **It sends the field's text to the AI provider you set up (Anthropic by default).** Who may use it, the text length and the number of requests per minute are checked on the server. See [Rewrite with AI](README.md#rewrite-with-ai).

- **IDs from 13 more countries in `IdInput`**: Brazil's CPF and CNPJ, Spain's NIF/NIE and CIF, Italy's Codice Fiscale and Partita IVA, France's SIREN and SIRET, the US EIN and ABA routing number, Australia's ABN, India's GSTIN, and SWIFT/BIC codes — each checked with its check digits (or, for SWIFT/BIC, its shape and country) and shown in the usual grouping. See [The ID field](README.md#the-id-field).
- **Durations: `DurationInput`**: type `1:30`, `1.5h`, `90m` or `1時間30分` and it is saved as whole minutes. `roundTo(15)`, `minMinutes()` / `maxMinutes()`, and `DurationColumn`, `DurationEntry` and the `DurationSum` total show the minutes as `1:30` or `1h 30m`. See [Durations](README.md#durations).
- **Range filters: `MoneyRangeFilter` and `NumberRangeFilter`**: filter a table by “from” and/or “to” an amount or a number, with a heading that names the column and indicators like “Total: $1,000–$5,000”. See [Range filters](README.md#range-filters).
- **Edit values in table rows: `MoneyInputColumn`, `NumberInputColumn` and `PhoneInputColumn`**: change a price, quantity or phone number right in the list. They save in the same format and check the same rules as `MoneyInput`, `NumberInput` and `PhoneInput`; a value that can't be saved is reported and put back. See [Editing values in table rows](README.md#editing-values-in-table-rows).
- **Show barcodes and QR codes: `BarcodeColumn` and `BarcodeEntry`**: draw a saved value as a QR code, Code 128, EAN-13, EAN-8, UPC-A or Code 39 on the server, as SVG. `BarcodeEntry::downloadable()` adds an SVG download. QR codes use `bacon/bacon-qr-code` (BSD-2-Clause), install it with `composer require bacon/bacon-qr-code` if you need QR codes; the other formats need nothing extra. See [Showing barcodes and QR codes](README.md#showing-barcodes-and-qr-codes).

### Fixed

- **Smart paste knows "today" in the panel's time zone**: the date given to the AI (to resolve "tomorrow", "next Friday") was the app's UTC date, so with the panel set to e.g. America/New_York it was a day ahead during the UTC night. It now uses the panel's time zone (`FilamentTimezone`).
- **`UrlInput::openButton()` stayed greyed out and did nothing in some places:** when the field's value arrives after the field is drawn (for example a row of a Warp Repeater table that turns into a real field when you hover it), the open button was disabled even though the field held a valid URL. The button now re-checks the value right after it is drawn, when the pointer reaches it, and when the field gets focus.

## [1.1.3] - 2026-11-20

### Fixed

- **Console error `warpInputMoney is not defined`**: on pages where Livewire re-renders an Input field inside another Alpine component, the browser console could show `warpInputMoney is not defined` before the component's script had arrived. Empty components are now registered up front, and the real ones replace them as soon as they load. No visible change.

## [1.1.2] - 2026-11-16

### Fixed

- **Static analysis**: Larastan no longer reports “Call to an undefined method” for `TextEntry::warpEditable()`. The plugin ships `extension.neon`, a PHPStan extension that reads Filament's macros (loaded automatically with `phpstan/extension-installer`; otherwise add it to `includes`). See “Static analysis” in the README.
- **Static analysis**: the setting methods of the plugin class are declared as returning the plugin, so `$panel->plugin(…::make()->…())` no longer fails with “Panel::plugin() expects Filament\Contracts\Plugin”.

## [1.1.1] - 2026-11-13

### Fixed

- **`TextEntry::warpEditable()`:** when someone else saved the same field within the same second as you opened it, your save went through and replaced their value. The check now compares the field's own value as well as `updated_at`, which only keeps whole seconds.

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
