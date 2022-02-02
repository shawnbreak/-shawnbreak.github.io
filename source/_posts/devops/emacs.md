---
title: emacs
---
# move
C-n, C-p: move by line
C-f, C-b: move by character
M-f, M-b: move by word
C-a, C-e: move to the start or end of the line 

C-v, M-v: move by screen
C-l: move the cursorline to the middle, top, bottom

# edit
C-w
C-d
C-j
C-k
C-y

# zoom
C-x C-+: zoom in
C-x C--: zoom out




# search
C-s
C-r

# file
C-x C-f

# directory
C-x d

# save
C-x s

# buffer
C-x C-b
C-x b <buffrename>
C-x k

# window
C-x 1
C-x 2
C-x 3
C-x o

C-M-v

# frames
C-x 5 2
C-x 5 0

# org-mode  



# init.el

runemacs -q -l init.el

```lisp

(setq inhibit-startup-message t)

(scroll-bar-mode -1)     ; Disable visibale scrollbar
(tool-bar-mode -1)       ; Disable the toolbar
(tooltip-mode -1)        ; Disable tooltips
(set-fringe-mode 10)     ; Give some breathing room

;; Set up the visible bell
(menu-bar-mode -1)

(setq visible-bell t)

(set-face-attribute 'default nil :font "Consolas" :height 150)


(load-theme 'tango-dark)

;; Make ESC quit prompts
(global-set-key (kbd "<escape>") 'keyboard-escape-quit)


;; (clomun-number-mode)
(global-display-line-numbers-mode t)

(dolist (mode '(shell-mode-hook
		term-mode-hook
		org-mode-hook))
  (add-hook mode (lambda () (display-line-numbers-mode 0))))

;; Initialize package sources
(require 'package)

(setq package-archives '(("melpa" . "https://mepa.org/packages/")
			 ("org" . "https://orgmode.org/elpa/")
			 ("elpa" . "https://elpa.gnu.org/packages/")))

(package-initialize)
(unless package-archive-contents
  (package-refresh-contents))

;; ;; Initialize use-package on non-Linux platforms
;; (unless (package-installed-p 'use-package)
;;  (package-install 'use-package))

;; (require 'use-package)
;; (setq use-package-always-ensure t)

;; (use-package command-log-mode)

```

# 多行编辑



| 列编辑快捷键 | 描述                                 |
| :----------- | :----------------------------------- |
| C-x r k      | 剪切一个矩形块                       |
| C-x r y      | 粘贴一个矩形块                       |
| C-x r o      | 插入一个矩形块                       |
| C-x r c      | 清除一个矩形块(使其变成空白)         |
| C-x r t      | 在选定区域的所有列前插入相同的字符串 |



| 快捷键 | 描述                                     |
| :----- | :--------------------------------------- |
| C-c f  | 按照语义扩大选中区域                     |
| C-c b  | 按照语义缩小选中区域                     |
| C-c i  | 选中该 buffer 中所有和选中区域相同的内容 |
