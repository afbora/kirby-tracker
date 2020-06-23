# Kirby Tracker WIP

Track content changes in Kirby 3.

## On privacy

**Before using this plugin, please think about the privacy implications.** You will be processing personal data of your users: their name, the date of their change and meta information about their change. This requires you to follow the privacy laws in your jurisdiction. Please consult a lawyer in case of doubt.

This plugin was initially conceived to create a dashboard for a private members area where all users consented to share their changes with the other users.

## Installation

### Download

Download and copy this repository to `/site/plugins/tracker`.

### Git submodule

```
git submodule add https://github.com/hananils/kirby-tracker.git site/plugins/tracker
```

### Composer

```
composer require hananils/kirby-tracker
```

## Tracked data

Tracker will create a SQLite datebase in `site/logs/tracker.sqlite`. It will create two tables:

### tracks

Tracker will create a log of all change in this database:

- `id`: the internal id
- `user`: the Kirby user id
- `datetime`: the timestamp of the change
- `kid`: the Kirby id of the current context (page id, user id, file id)
- `model`: the model of the current context (page, user, file)
- `action`: the name of the action, referring to the Kirby hook, see https://getkirby.com/docs/reference/plugins/hooks
- `changes`: meta information about the change, e. g. the field names affected or the page status before and after

### notifications

Tracker will also create a list of all related pages, users and files that are affected by a change:

- `id`: the internal id
- `kid`: the related Kirby id
- `datetime`: the timestamp of the change
- `track`: the tracked change
- `status`: whether the page, user or file has been added, untouched or removed as reference

## Usage

### $site->tracks($limit)

Returns a `TracksCollection` of all changes of all users, defaults to 20 entries.

### $site->notifications($limit)

Returns a `TracksCollection` of all notifications, defaults to 20 entries.

### $page->tracks($limit)

Returns a `TracksCollection` of all changes on the given page, defaults to 20 entries.

### $page->notifications($limit)

Returns a `TracksCollection` of all notifications for the given page, defaults to 20 entries.

### $user->tracks($limit)

Returns a `TracksCollection` of all changes for the given user, defaults to 20 entries.

### $user->notifications($limit)

Returns a `TracksCollection` of all notifications for the given user, defaults to 20 entries.

### TracksCollection

The plugin provides a `TracksCollection` you can use to query, filter and output tracking information:

```php
$tracks = new Hananils\TracksCollection();
$notifications = new Hananils\TracksCollection('notifications');
```

#### filterBy($column, $comparison, \$value)

Applies a filter to the output.

#### limit(\$limit)

Limits the number of returned tracks, defaults to 20.

#### offset(\$offset)

Applies and offset to the result.

#### sortBy()

Sorts the results.

#### validate()

Makes sure that only tracks for existing pages, users and files are returned, excluding those deleted in the meantime.

#### toArray()

Returns the result as an array

### Track

Each item of the `TrackCollection` is a `Track` object:

#### isValid()

Returns `true` is the referenced Kirby object exists, otherwise `false`.

#### toUser()

Returns the Kirby user object.

#### toReference()

Returns the Kirby object of the given reference, either the changed page, user or file.

#### toDate(\$format)

Returns either the `DateTime` representation of the track or – if a format was provided – the formatted date.

#### toTrack()

Returns the related track for a notification.

#### toStatus()

Returns the status of a notified reference, either `added`, `unchanged` or `removed`.

#### value()

Return the value of the track.

#### toArray()

Returns an array representation of the track.

## To Do

- Create Panel sections for tracks and notifications
- Make `TracksCollection` use Kirby's collection classes.
- Make `TracksCollection` groupable by fields.

## License

MIT

## Credits

- hana+nils · Büro für Gestaltung

```

```
