---
layout: post
title:  "反转二叉树"
subtitle: "使用scheme来实现的反转二叉树"
date:   2017-06-04 10:45:13 -0400
background: '/img/posts/06.jpg'
---

最近在研究lisp，写个反转二叉树来练练手，代码如下

{% highlight scheme %}

(define (tree-top tree) (car tree))

(define (tree-left tree) (car (cdr tree)))

(define (tree-right tree) (car (cdr (cdr tree))))

(define (atom? x)
  (and (not (null? x))
              (not (pair? x))))


(define (revert-tree tree)
  (cond
   ((and (atom? (tree-left tree)) (atom? (tree-right tree)))
    (cons (tree-top tree)
          (cons (tree-right tree)
                (cons (tree-left tree)
                      '()))))
   (else
    (cons (tree-top tree)
          (cons (revert-tree (tree-right tree))
                (cons (revert-tree (tree-left tree))
                      '()))))))

(define my-tree (list 1 (list 11 111 112) (list 12 121 122)))
(revert-tree my-tree)
{% endhighlight %}

