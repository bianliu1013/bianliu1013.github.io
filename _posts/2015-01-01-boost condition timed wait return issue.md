---
layout: post
title: boost - condition timed_wait return issue
date:  2015-01-01 11:23:32
categories: boost c++
---


在重写驱动时候，发现程序莫名其妙的copy占用率奇高，一步步的调试，发现是一个线程的在不停的空转，最后定位到
<pre><code>
boost::mutex::scoped_lock lock(mutex);
boost::system_time timeout = boost::get_system_time() + boost::posix_time::seconds(1);
if(condition.timed_wait(lock, timeout)
{
    //log
}
else
{
    //log
}
</code></pre>
改为以下用法，问题解决。
<pre><code>
boost::mutex::scoped_lock lock(mutex);
//boost::system_time timeout = boost::get_system_time() + boost::posix_time::seconds(1);
if(condition.timed_wait(lock, boost::posix_time::milliseconds(1000)))
{
    //log
}
else
{
    //log
}
</code></pre>

原因不明， google上说有可能是相对时间引起的， 但是普遍推荐的是第一种方法。 另一个项目用此方法也从来没有出现在过这个问题，不解。。。

留个记号， 以后调查。


后续篇
翻开boost源码： boost\thread\win32\condition_variable.hpp

<pre><code>
bool timed_wait(unique_lock<mutex>& m,boost::system_time const& wait_until)
{
    return do_wait(m,wait_until);
}

bool timed_wait(unique_lock<mutex>& m,boost::xtime const& wait_until)
{
    return do_wait(m,system_time(wait_until));
}
template<typename duration_type>
bool timed_wait(unique_lock<mutex>& m,duration_type const& wait_duration)
{
    return do_wait(m,wait_duration.total_milliseconds());
}
</code></pre>


condition.timed_wait(lock, timeout) will call timed_wait(unique_lock<mutex>& m,boost::system_time const& wait_until)

condition.timed_wait(lock, boost::posix_time::milliseconds(500))) will call timed_wait(unique_lock<mutex>& m,duration_type const& wait_duration)


有理由怀疑 在调用的do_wait的时候，时间算错了？ boost::system_time 转换为 timeout，  tick_type 转换为 timeout。
查看timeout的定义： boost\thread\win32\thread_data.hpp


<pre><code>
struct timeout
{
    unsigned long start;
    uintmax_t milliseconds;
    bool relative;
    boost::system_time abs_time;

    static unsigned long const max_non_infinite_wait=0xfffffffe;

    timeout(uintmax_t milliseconds_):
        start(win32::GetTickCount()),
        milliseconds(milliseconds_),
        relative(true),
        abs_time(boost::get_system_time())
    {}

    timeout(boost::system_time const& abs_time_):
        start(win32::GetTickCount()),
        milliseconds(0),
        relative(false),
        abs_time(abs_time_)
    {}
}；
</code></pre>

<br>
哦哦哦，一个是相对时间， 一个是绝对时间， 再看timeout的成员函数：
<br>
<pre><code>
remaining_time remaining_milliseconds() const
{
    if(is_sentinel())
    {
        return remaining_time(win32::infinite);
    }
    else if(relative)
    {
        unsigned long const now=win32::GetTickCount();
        unsigned long const elapsed=now-start;
        return remaining_time((elapsed<milliseconds)?(milliseconds-elapsed):0);
    }
    else
    {
        system_time const now=get_system_time();
        if(abs_time<=now)
        {
            return remaining_time(0);
        }
        return remaining_time((abs_time-now).total_milliseconds()+1);
    }
}
</code></pre>

<br>
可以怀疑，我们在用boost::get_system_time() 的时候，出了什么小case， 调整代码：
<br>
<pre><code>
boost::system_time timeout = boost::get_system_time() + boost::posix_time::seconds(1);
boost::system_time now = boost::get_system_time();
boost::detail::timeout to(timeout);
long rm = to.remaining_milliseconds().milliseconds;
//print rm;
</code></pre>

<br>
这时候发生悲剧， 程序莫名其妙的好了， rm返回值维持在 remaining_milliseconds = 1001，而不是我期望的0 or 非法值。

anyway，下次碰到继续跟踪，有进展就好。