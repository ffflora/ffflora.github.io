---
title: "How to Use Auto Completion in GCP"
date: 2019-08-28T21:30:55-07:00
draft: false
toc: false
images:
tags:
- GCP
categories:	
- Data Science
---

## Auto-completion

`gcloud interactive` has auto prompting for commands and flags, and displays inline help snippets in the lower section as the command is typed.

Static information, like command and sub-command names, and flag names and enumerated flag values, are auto-completed using dropdown menus.

Install the beta components:

```python
gcloud components install beta
```

Enter the gcloud interactive mode:

```
gcloud beta interactive
```

When using the interactive mode, click on the **Tab** key to complete file path and resource arguments. If a dropdown menu appears, use the **Tab** key to move through the list, and the **Space bar** to select your choice.

Try it out! Start typing the following command, using auto-complete to finish the command:

```
gcloud compute instances describe <your_vm>
```

Across the bottom of Cloud Shell you can see the shortcut to toggles this feature. Try out the F2 toggle:

`F2`: help: STATE Toggles the active help section, ON when enabled, OFF when disabled.

