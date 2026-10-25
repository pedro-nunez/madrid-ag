# Algebraic Geometry in Madrid

A list of algebraic geometry conferences, workshops, and seminars in Madrid.

**Live site:** https://pedro-nunez.github.io/madrid-ag/

## Adding an event

Use the [Add Event](https://pedro-nunez.github.io/madrid-ag/add-event/)
page, also reached from the homepage's **Add Event** button. Choose
**Meeting** or **Talk** to open the corresponding GitHub issue form:

- `.github/ISSUE_TEMPLATE/add-meeting.yml` asks for a title, institution,
  and start date, with optional end date, location, homepage, and a
  checkbox for meetings organized away from the institution's campus.
- `.github/ISSUE_TEMPLATE/add-talk.yml` asks for a talk title, speaker,
  hosting institution, and date, with optional institution abbreviation,
  seminar name, seminar website, location, and talk homepage.

Both forms create issues labeled `add-event`; their titles identify
them as `[Meeting]` or `[Talk]`. A maintainer reviews each suggestion,
copies accepted details into the appropriate data file, and closes the
issue. This process is manual; the YAML files are the source of truth.
Use `category: meeting` in `_data/meetings.yml` or `category: talk` in
`_data/talks.yml`, according to the chosen form. For talks, copy
**Institution abbreviation** to `institution_abbreviated` and
**Seminar website** to `seminar_url`. For meetings, a checked
**Organized at a different venue** option becomes `organized_by: true`.

## Event data

Listings live in `_data/meetings.yml` and `_data/talks.yml`. The homepage
combines both lists and renders them through `_includes/events-list.html`.
Every entry must explicitly include the appropriate `category`.

### Meetings

Conferences, workshops, minicourses, summer schools, and similar programs
belong in `_data/meetings.yml`. Required fields are `title`,
`category: meeting`, `institution`, and `start`:

```yaml
- title: "Algebraic Geometry Workshop"
  category: meeting
  institution: "Instituto de Ciencias Matemáticas (ICMAT)"
  start: 2026-11-05
  end: 2026-11-06      # optional; omit for a single-day meeting
  location: "Madrid" # optional city or geographical region
  url: "https://example.org/workshop" # optional meeting homepage
  organized_by: true # optional; only for meetings away from its campus
```

Meeting cards have a blue background that adapts to the site's theme.
The first line shows the
date and linked title. The second reads **At [institution]**, followed
by **in [location]** when supplied. Use `organized_by: true` only when
the institution organizes a meeting at a different physical venue;
this changes **At** to **Organized by**. Otherwise omit it.

### Talks

Individual seminar talks, colloquium talks, and other standalone
presentations belong in `_data/talks.yml`. Required fields are `title`
(the individual talk's title), `speaker`, `category: talk`,
`institution` (the full hosting name), and `start`:

```yaml
- title: "Moduli spaces of sheaves"
  speaker: "Jane Doe"
  category: talk
  institution: "Universidad Complutense de Madrid"
  institution_abbreviated: "UCM" # optional short name for the card
  start: 2026-11-05
  location: "Madrid" # optional city or geographical region
  seminar: "Algebraic Geometry Seminar" # optional recurring series
  seminar_url: "https://example.org/seminar" # optional series website
  url: "https://example.org/talk" # optional individual talk page
```

Talk cards show **date: seminar (institution_abbreviated, location)**
on the first line, followed by **speaker — talk title** on the second.
If `institution_abbreviated` is omitted, empty, or only whitespace, the
card uses the full `institution` instead. Always keep the full name in
`institution`; it remains searchable when an abbreviation is displayed.

Omit `seminar` for a standalone talk; the heading defaults to
**Seminar talk**. The seminar name links to `seminar_url`, and the talk
title independently links to `url`. Supply a seminar name whenever
you supply `seminar_url`. Repeat series details for each dated talk.
Omit meeting-only fields `end` and `organized_by`.

Replace the initial `[]` with the first talk entry; keep `[]` when there
are no talks. Classify what an entry represents, independently of its
duration: a one-day workshop is a meeting; an individual presentation
listed separately within a workshop is a talk.

The examples above illustrate the format and are not actual listings.
Omit unknown optional fields. For both categories, `location` is a city
or geographical region, separate from the institution; omitting it
also omits the surrounding location punctuation.

### Dates and ordering

Dates use `YYYY-MM-DD`. Entries can appear in any order in either file.
At each site build, an event belongs to **Upcoming Events** if its `end`
(or `start`, when there is no `end`) is today or later in
`Europe/Madrid`. Other events appear in **Past Events**. A meeting
stays upcoming through its final day.

Within each section, both event types are sorted by `start`: earliest
first for Upcoming, most recent first for Past. Ordering is independent
of the order of entries in the data files.

### Searching events

The homepage's **Search** button opens filters for:

- Text displayed in each card, including titles, speakers, seminars,
  institutions, and locations. Institution searches also match the
  full name when an abbreviation is displayed.
- Event type: meeting or talk. Selecting neither or both shows both.
- A range for the event's starting date, inclusive of both endpoints;
  either endpoint can be left blank.

The filters work together and preserve the existing event order.
**Clear filters** restores the full list. An invalid date range shows
an explanation, and a valid search with no matches shows an empty message.

## Local development

Requires Ruby and Bundler. This is a [Jekyll](https://jekyllrb.com/) site
using the `github-pages` gem to match GitHub Pages' build environment.

```sh
bundle config set --local path vendor/bundle
bundle install
bundle exec jekyll serve
```

The first command configures this checkout's gems to install in
`vendor/bundle`, avoiding system gem directory permissions. Run it once
per checkout; the local configuration and installed gems are ignored by Git.

Then open http://localhost:4000/madrid-ag/.
