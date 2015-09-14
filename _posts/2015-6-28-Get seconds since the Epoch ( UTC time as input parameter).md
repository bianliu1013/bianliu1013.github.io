---
layout: post
title: Get seconds since the Epoch ( UTC time as input parameter)  !
date:  2015-05-28 11:23:32
categories: linux c++
---


### code:
<pre><code>
/*
 returns the utc timezone offset
 (e.g. -8 hours for PST)
 */
int get_utc_offset() {
    time_t zero = 24*60*60L;
    struct tm * timeptr;
    int gmtime_hours;
    
    /* get the local time for Jan 2, 1900 00:00 UTC */
    timeptr = localtime( &zero );
    gmtime_hours = timeptr->tm_hour;
    
    /* if the local time is the "day before" the UTC, subtract 24 hours
     from the hours to get the UTC offset */
    if( timeptr->tm_mday < 2 )
        gmtime_hours -= 24;
    
    return gmtime_hours;
}

/*
 the utc analogue of mktime,
 (much like timegm on some systems)
 */
time_t tm_to_time_t_utc( struct tm * timeptr ) {
    
    /* gets the epoch time relative to the local time zone,
     and then adds the appropriate number of seconds to make it UTC */
    return mktime( timeptr ) + get_utc_offset() * 3600;
}


time_t mktime_wrapper( int month, int day, int year,
                      int hour=0, int min=0, int sec=0, bool isDST=0
                      )
{
    tm t;
    t.tm_sec=sec, t.tm_min=min, t.tm_hour=hour, t.tm_isdst=isDST;
    t.tm_mday=day, t.tm_mon=month-1, t.tm_year=year-1900;

    //tm tt = tm_to_time_t_utc(&t);
    return tm_to_time_t_utc(&t);
}



To TEST:
printf("seconds since the Epoch: %ld\n", mktime_wrapper(1, 1, 1970));

</code></pre>


**Note**:
    pay attention to mac, the timezone has "0.5" hour sense.  shit. 