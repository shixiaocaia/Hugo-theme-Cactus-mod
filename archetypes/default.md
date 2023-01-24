---
title: "默认标题"
date: {{ .Date }}
description: 
tags:
  - 闲话长说
slug: {{ substr (md5 (printf "%s%s" .Date (replace .TranslationBaseName "-" " " | title))) 4 8 }}
---

