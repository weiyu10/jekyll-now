---
layout: post
title: 反转二叉树
---

{% highlight scheme %}
(define my-tree (list 1 (list 11 111 112) (list 12 121 122)))

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

(revert-tree my-tree)
{% endhighlight %}

