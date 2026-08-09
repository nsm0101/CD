# CD is superseded by CloseDose

> **This repository is no longer developed.** The parent-facing CloseDose site
> now lives in [`nsm0101/CloseDose`](https://github.com/nsm0101/CloseDose) under
> [`public/`](https://github.com/nsm0101/CloseDose/tree/main/public) and is
> served at <https://closedose.com/>.

This repository held an earlier iteration of the parent-facing site. Everything
still maintained has a successor in the main repository:

| Here | Now |
| --- | --- |
| `index.html` and the dosing calculator | [`public/index.html`](https://github.com/nsm0101/CloseDose/blob/main/public/index.html) with [`public/widget/close-dose-calculator.js`](https://github.com/nsm0101/CloseDose/blob/main/public/widget/close-dose-calculator.js) |
| `index-es.html`, `index-fr.html`, `index-pt.html` | Runtime translation via [`public/i18n.mjs`](https://github.com/nsm0101/CloseDose/blob/main/public/i18n.mjs) and `public/i18n/` |
| `about.html`, `contact.html`, `donations.html` | The matching pages under `public/` |
| `Ibuprofen/`, `Donations/`, `PoisonHelp/`, `Upandaway/` | `public/Ibuprofen/`, `public/Donations/`, and related asset directories |
| `dosing-verification.md`, `IMPLEMENTATION-SUMMARY.md` | Superseded, see below |

## Dosing verification

The AAP/FDA age-band correction documented in `IMPLEMENTATION-SUMMARY.md` is
carried forward. The current calculator splits pediatric and adolescent dosing
the same way, and applies a tighter adolescent ibuprofen ceiling of 600 mg
rather than the 800 mg recorded here. Nothing in that work is lost by retiring
this repository.

## Where to make changes

Open pull requests against `nsm0101/CloseDose`. Changes made here will not reach
production and will not be merged back.

This repository is kept read-only for history and can be archived.

## Logo Files and License

The CloseDose logo files in this repository are the intellectual property of
Nickolas Mancini, MD, MBA and are provided solely for use with the CloseDose
project. Redistribution or modification of the logo assets is prohibited without
express permission. See LOGO_LICENSE.md for the full license.
