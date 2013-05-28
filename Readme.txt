I am so happy that you also was trying to replicate the paper. I used SAS package to test thier paper, but for more clear understnding each step, I used Excel too. Here is my approach.

1. Sample data
We need two sample data; DJIT and Google trends for 'debt' term. I collected DJIT data from http://www.djaverages.com/?go=industrial-index-data&report=performance with index price history from Jan 5, 2004 to Feb 22, 2011. I drawed a graph of DJIT and get the same result with Figure 1. I think that DJIT data doesn't have any problem issue.

I collected the trend of 'debt' from Google Trends, http://www.google.com/trends/explore?q=debt#q=debt&geo=US&date=1%2F2004%2086m&cmpt=q. I restricted my search range with 'debt' term, U.S. geography, and the period from Jan 2004 to Feb 2011. I downloaded this result with 'Download as csv' in the same site. Here is first six result.

Week	debt
2004-01-04 - 2004-01-10	63
2004-01-11 - 2004-01-17	66
2004-01-18 - 2004-01-24	63
2004-01-25 - 2004-01-31	66
2004-02-01 - 2004-02-07	61
2004-02-08 - 2004-02-14	62

2. matching DJIT data and debt data
I merged debt data with DJIT by including the nearest date of debt to the date of DJIT. For instance, the end date of the first week is Jan 10, 2004, and this debt data is matched with the first occurance of djit after Jan 10. Because of holidays, some weeks doesn't have transaction on Monday of a week. So, we need to find the first transaction day after the end day of a week of the term data. Here is the first several lines.

Week	debt	sdate	edate	ddate	djit
2004-01-04 - 2004-01-10	63	1/4/2004	1/10/2004	1/12/2004	10485.17702
2004-01-11 - 2004-01-17	66	1/11/2004	1/17/2004	1/20/2004	10528.6635
2004-01-18 - 2004-01-24	63	1/18/2004	1/24/2004	1/26/2004	10702.51163
2004-01-25 - 2004-01-31	66	1/25/2004	1/31/2004	2/2/2004	10499.18265
2004-02-01 - 2004-02-07	61	2/1/2004	2/7/2004	2/9/2004	10579.03279
2004-02-08 - 2004-02-14	62	2/8/2004	2/14/2004	2/17/2004	10714.88173
2004-02-15 - 2004-02-21	61	2/15/2004	2/21/2004	2/23/2004	10609.62473
2004-02-22 - 2004-02-28	62	2/22/2004	2/28/2004	3/1/2004	10678.14178

3. Moving average
When Google Trends report trends data, it seems not to confirm eactly on Sunday. Suppose that you collect trend data on Jan 11, 2004 for the week 2004-01-04 to 2004-01-10. On Sunday (Jan 11), you may see "Partial Data" accourding to Google Trends because Goodle trends finalized trends data for the previous week. Note: Google uses a week from Sunday to Saturday. In page 2 of the paper, the authors states that "where Google defines weeks as ending on a Sunday.." I think this is not correct.

Because of the incomplete data on Sunday, the authors make a moving average with the last 3 days. Thus, we need to make an average with three days. The first row could be just one, and the second row could be the average of the first and second. Here is the first few rows. debt3 indicates the aveage of three days.

Week	debt	sdate	edate	ddate	djit	debt3
2004-01-04 - 2004-01-10	63	1/4/2004	1/10/2004	1/12/2004	10485.17702	63
2004-01-11 - 2004-01-17	66	1/11/2004	1/17/2004	1/20/2004	10528.6635	64.5
2004-01-18 - 2004-01-24	63	1/18/2004	1/24/2004	1/26/2004	10702.51163	64
2004-01-25 - 2004-01-31	66	1/25/2004	1/31/2004	2/2/2004	10499.18265	65
2004-02-01 - 2004-02-07	61	2/1/2004	2/7/2004	2/9/2004	10579.03279	63.33333333
2004-02-08 - 2004-02-14	62	2/8/2004	2/14/2004	2/17/2004	10714.88173	63
2004-02-15 - 2004-02-21	61	2/15/2004	2/21/2004	2/23/2004	10609.62473	61.33333333
2004-02-22 - 2004-02-28	62	2/22/2004	2/28/2004	3/1/2004	10678.14178	61.66666667

4. delta
delta = n(t) - debt3(t-1)

Here are few first rows.

Week	debt	sdate	edate	ddate	djit	debt3	delta
2004-01-04 - 2004-01-10	63	1/4/2004	1/10/2004	1/12/2004	10485.17702	63	0
2004-01-11 - 2004-01-17	66	1/11/2004	1/17/2004	1/20/2004	10528.6635	64.5	3
2004-01-18 - 2004-01-24	63	1/18/2004	1/24/2004	1/26/2004	10702.51163	64	-1.5
2004-01-25 - 2004-01-31	66	1/25/2004	1/31/2004	2/2/2004	10499.18265	65	2
2004-02-01 - 2004-02-07	61	2/1/2004	2/7/2004	2/9/2004	10579.03279	63.33333333	-4
2004-02-08 - 2004-02-14	62	2/8/2004	2/14/2004	2/17/2004	10714.88173	63	-1.333333333
2004-02-15 - 2004-02-21	61	2/15/2004	2/21/2004	2/23/2004	10609.62473	61.33333333	-2
2004-02-22 - 2004-02-28	62	2/22/2004	2/28/2004	3/1/2004	10678.14178	61.66666667	0.666666667
2004-02-29 - 2004-03-06	61	2/29/2004	3/6/2004	3/8/2004	10529.4783	61.33333333	-0.666666667
2004-03-07 - 2004-03-13	61	3/7/2004	3/13/2004	3/15/2004	10102.89483	61.33333333	-0.333333333

5. trading and return
if delta(t-1) > 0 then take short position.
if delta(t-1> < 0 then take long position.
if delte(t-1) = 0 then no action.
I think the authors take no action for the tie break. When they explained transaction fees, they pointed out the maximum number of transaction is only 104. If they take short or long for the tie break, they should described the number of trasctions is 104, rather than the maximum number.

For the short position, the return is log(p_t-1) - log(p_t), and for the long position, the return is log(p_t) - log(p_t-1).

Here are the first few rows.

Week	debt	sdate	edate	ddate	djit	debt3	delta	ret
2004-01-04 - 2004-01-10	63	1/4/2004	1/10/2004	1/12/2004	10485.17702	63	0	0
2004-01-11 - 2004-01-17	66	1/11/2004	1/17/2004	1/20/2004	10528.6635	64.5	3	0
2004-01-18 - 2004-01-24	63	1/18/2004	1/24/2004	1/26/2004	10702.51163	64	-1.5	-0.016377051
2004-01-25 - 2004-01-31	66	1/25/2004	1/31/2004	2/2/2004	10499.18265	65	2	-0.019181035
2004-02-01 - 2004-02-07	61	2/1/2004	2/7/2004	2/9/2004	10579.03279	63.33333333	-4	-0.007576593
2004-02-08 - 2004-02-14	62	2/8/2004	2/14/2004	2/17/2004	10714.88173	63	-1.333333333	0.012759588
2004-02-15 - 2004-02-21	61	2/15/2004	2/21/2004	2/23/2004	10609.62473	61.33333333	-2	-0.009872009
2004-02-22 - 2004-02-28	62	2/22/2004	2/28/2004	3/1/2004	10678.14178	61.66666667	0.666666667	0.006437245
2004-02-29 - 2004-03-06	61	2/29/2004	3/6/2004	3/8/2004	10529.4783	61.33333333	-0.666666667	0.014020047
2004-03-07 - 2004-03-13	61	3/7/2004	3/13/2004	3/15/2004	10102.89483	61.33333333	-0.333333333	-0.04135678

6. Cumulating return
We have two ways to cumulating returns: summing from the beginning to the ending, and geometric returns (1+r)(1+r)....
In my case, the geometric returns are 2.027 and the sum of the returns is 0.824.

Here are the first and last few rows.
Week	debt	sdate	edate	ddate	djit	debt3	delta	ret	sret	cret
2004-01-04 - 2004-01-10	63	1/4/2004	1/10/2004	1/12/2004	10485.17702	63	0	0	0	1
2004-01-11 - 2004-01-17	66	1/11/2004	1/17/2004	1/20/2004	10528.6635	64.5	3	0	0	1
2004-01-18 - 2004-01-24	63	1/18/2004	1/24/2004	1/26/2004	10702.51163	64	-1.5	-0.016377051	-0.016377051	0.983622949
2004-01-25 - 2004-01-31	66	1/25/2004	1/31/2004	2/2/2004	10499.18265	65	2	-0.019181035	-0.035558085	0.964756044
2004-02-01 - 2004-02-07	61	2/1/2004	2/7/2004	2/9/2004	10579.03279	63.33333333	-4	-0.007576593	-0.043134678	0.95744648
2004-02-08 - 2004-02-14	62	2/8/2004	2/14/2004	2/17/2004	10714.88173	63	-1.333333333	0.012759588	-0.03037509	0.969663103
2004-02-15 - 2004-02-21	61	2/15/2004	2/21/2004	2/23/2004	10609.62473	61.33333333	-2	-0.009872009	-0.040247099	0.96009058
2004-02-22 - 2004-02-28	62	2/22/2004	2/28/2004	3/1/2004	10678.14178	61.66666667	0.666666667	0.006437245	-0.033809854	0.966270918
2004-02-29 - 2004-03-06	61	2/29/2004	3/6/2004	3/8/2004	10529.4783	61.33333333	-0.666666667	0.014020047	-0.019789806	0.979818082
....
2011-01-16 - 2011-01-22	63	1/16/2011	1/22/2011	1/24/2011	11980.51975	62.66666667	7.666666667	-0.011972994	0.835101399	2.050606652
2011-01-23 - 2011-01-29	67	1/23/2011	1/29/2011	1/31/2011	11891.93241	63	4.333333333	0.007421755	0.842523154	2.065825752
2011-01-30 - 2011-02-05	61	1/30/2011	2/5/2011	2/7/2011	12161.62995	63.66666667	-2	-0.022425689	0.820097466	2.019498187
2011-02-06 - 2011-02-12	61	2/6/2011	2/12/2011	2/14/2011	12268.19208	63	-2.666666667	0.008723994	0.828821459	2.037116276
2011-02-13 - 2011-02-19	67	2/13/2011	2/19/2011	2/22/2011	12212.79189	63	4	-0.004525986	0.824295473	2.027896317



