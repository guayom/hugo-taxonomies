---
title: "{{ replace .TranslationBaseName "-" " " | title }}"
description: "This is the description for {{ replace .TranslationBaseName "-" " " | title }}"
brands:
  - {{ index (split .TranslationBaseName "-") 0 }}
date: {{ .Date }}
draft: false
---
