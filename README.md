# AB-test
#under the null hypothesis the convertion rate 11.96%
p_new = df2.converted.mean()
p_new
#under the null hypothesis the convertion rate 11.96%
p_old = df2.converted.mean()
p_old
#the number of individuals who converted to new page is 145310
n_new = df2.query("landing_page == 'new_page'")['converted'].count()
n_new
#the number of individuals who converted to new page is 145274
n_old = df2.query("landing_page == 'old_page'")['converted'].count()
n_old
#uses np.random.binomial to Simulate n_new transactions with a convert rate of p_new under the null hypothesis
new_page_converted = np.random.binomial(1, p_new, n_new)
new_page_converted.mean()
#uses np.random.binomial to Simulate n_old transactions with a convert rate of p_old under the null hypothesis
old_page_converted = np.random.binomial(1, p_old, n_old)
old_page_converted.mean()
#find the difference in the observed simulation
simulation_diff = new_page_converted.mean() - old_page_converted.mean()
print('The difference in the observed simulation: {}'.format(simulation_diff.mean()))
#uses np.random.binomial to Simulate 10,000  p_new - p_old values
new_converted_simulation = np.random.binomial(n_new, p_new, 10000)/n_new
old_converted_simulation = np.random.binomial(n_old, p_old, 10000)/n_old
p_diffs = new_converted_simulation - old_converted_simulation
p_diffs = np.array(p_diffs)
old_converted_simulation.mean(), new_converted_simulation.mean(), p_diffs.mean()
#find the actual difference for each group
control_convert_actual = df2.query('group == "control"')['converted'].mean()
treatment_convert_actual = df2.query('group == "treatment"')['converted'].mean()
observed_diff = treatment_convert_actual - control_convert_actual
# view the confidence interval
p_diffs = np.array(p_diffs)
low, upper = np.percentile(p_diffs, 2.5), np.percentile(p_diffs, 97.5)
low, upper
plt.hist(p_diffs);
plt.axvline(x=low, color='g', linewidth=2);
plt.axvline(x=upper, color='g', linewidth=2);
plt.axvline(observed_diff, color='r', linewidth=2);
#find proportion of values greater than observed_diff since alternative hypothesis > null hypothesis
print("The actual observed difference is: {}".format(observed_diff))
print("The proportion of p_diffs greater than observed (p-value): {}".format((p_diffs > observed_diff).mean()))
