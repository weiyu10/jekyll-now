---
layout: post
title:  "google meglev 一致性hash算法"
subtitle: "python简略版"
date:   2017-11-22 10:00:00 -0400
background: '/img/posts/03.jpg'
---

# overview

最近看google的[maglev](http://static.googleusercontent.com/media/research.google.com/zh-CN//pubs/archive/44824.pdf)论文，自己尝试用python实现的一致性hash

{% highlight python %}
#!/usr/bin/env python


from hashlib import md5
from struct import unpack_from

def hash(key):
    key = str(key)
    hsh = unpack_from('>I', md5(str(key)).digest())[0]
    return hsh


class Maglev_hash():
    def __init__(self, rs):
        self.rs = rs
        self.rs_count = len(rs)
        self.vnode_count = 7
        self.permutation = []
        self.entry = []
        self.init_permutation()
        self.init_entry()

    def init_permutation(self):
        for rs_id in range(self.rs_count):
            self.permutation.append([])
            for vnode_id in range(self.vnode_count):
                self.permutation[rs_id].append(0)

        for rs_id in range(self.rs_count):
            for vnode_id in range(self.vnode_count):
                offset = hash(self.rs[rs_id]) % self.vnode_count
                skip = hash(self.rs[rs_id]) % (self.vnode_count - 1) + 1
                self.permutation[rs_id][vnode_id] = (offset + vnode_id*skip) % self.vnode_count
           
    def init_entry(self):
        self.entry = []
        entry_add_success = 0
        for vnode_id in range(self.vnode_count):
            self.entry.append(-1)

        for vnode_id in range(self.vnode_count):
            for rs_id in range(self.rs_count):
                except_vnode_id = self.permutation[rs_id][vnode_id]
                if entry_add_success == self.vnode_count:
                    break
                for retry in range(self.vnode_count):
                    if self.entry[except_vnode_id] == -1:
                        self.entry[except_vnode_id] = self.rs[rs_id]
                        entry_add_success = entry_add_success + 1
                        break
                    else:
                        except_vnode_id = except_vnode_id + 1
                    if except_vnode_id == self.vnode_count:
                        except_vnode_id = 0
  
    def echo(self):
        print self.entry


    def get_rs(self, key):
        vnode_id = hash(str(key)) % self.vnode_count
        return self.entry[vnode_id]


rs=['b1','b2','b3','b4']
rs2=['b1','b2','b3','b4','b10']
entry = Maglev_hash(rs) 
entry2 = Maglev_hash(rs2) 
print entry.echo()
print entry2.echo()

result = {}
for i in rs:
    result[i] = 0

moved = 0
for i in xrange(10000):
    rs1 = entry.get_rs(i)
    rs2 = entry2.get_rs(i)
    if rs1 != rs2:
        moved = moved + 1

print moved/float(10000)
    

{% endhighlight %}


# REF

<http://static.googleusercontent.com/media/research.google.com/zh-CN//pubs/archive/44824.pdf>

<http://xnhp0320.github.io/2016/06/26/Maglev-consistent-hash/>
