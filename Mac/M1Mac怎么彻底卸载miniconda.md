---
title: M1mac怎么彻底卸载miniconda
date: 2021-3-11 21:12:00
categories: 
cover: /img/p6.jpg
tags: tips
---





```shell
# Remove the root prefix
rm -rf $(conda info --base)

# Configuration file, common between conda installations
rm ~/.condarc
# Environment locations and system information
rm -rf ~/.conda
```

