# Meditation on Principles Notifier

A simple MacOS LaunchAgent Python script that shows you a popup with a new life principle every Monday morning at 9am. The principles are sourced from [Nabeel S. Qureshi's principles](https://nabeelqu.co/principles).

## What it does
- Shows a weekly popup with a new principle/insight
- Cycles through the principles in order
- Runs automatically in the background
- Starts on Monday mornings at 9am

## Requirements
- macOS (any recent version)

## Installation
1. Update the `USERNAME` and `PYTHON_PATH` in the `com.dagmartimler.meditationnotifier.plist` file:

```bash
sed -i '' "s/<USERNAME>/$(whoami)/g" com.dagmartimler.meditation_notifier.plist
sed -i '' "s|<PYTHON_PATH>|$(which python3)|g" com.dagmartimler.meditation_notifier.plist
```

2. Copy the files to your computer under `Library`:

```bash
cp meditation_notifier.py ~/Library/Scripts/
cp com.dagmartimler.meditation_notifier.plist ~/Library/LaunchAgents/
```

3. Register the service:

```bash
launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/com.dagmartimler.meditation_notifier.plist
```

4. Enable the service:

```bash
launchctl enable gui/$(id -u)/com.dagmartimler.meditation_notifier
```

5. Check if the service is running:

```bash
launchctl list | grep com.dagmartimler.meditation_notifier
```

## Run the script manually

Once the service is installed, you can run the kickstart command manually to test it:

```bash
launchctl kickstart gui/$(id -u)/com.dagmartimler.meditation_notifier
```

This will also run the script directly.

```bash
python ~/Library/Scripts/meditation_notifier.py
```

## Uninstall

```bash
launchctl bootout gui/$(id -u) ~/Library/LaunchAgents/com.dagmartimler.meditation_notifier.plist
rm ~/Library/Scripts/meditation_notifier.py
rm ~/Library/LaunchAgents/com.dagmartimler.meditation_notifier.plist
```

## Troubleshooting

Check the logs in `/tmp/meditationnotifier.log` and `/tmp/meditationnotifier.error.log` for more information.

```bash
tail -f /tmp/meditationnotifier.log
tail -f /tmp/meditationnotifier.error.log
```

## How it works
- The script keeps track of your start date
- Each week it shows a new principle from the collection
- The notification includes a week counter and the current principle
- Settings and progress are stored in `~/.meditation-notifier/`