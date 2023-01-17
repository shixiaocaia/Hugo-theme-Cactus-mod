---
title: "默认标题"
date: {{ .Date }}
description: 。
tags:
  - 默认Tags
categories:
  - 默认分类
slug: {{ substr (md5 (printf "%s%s" .Date (replace .TranslationBaseName "-" " " | title))) 4 8 }}
---

