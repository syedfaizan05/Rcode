install.packages('ggplot2')
install.packages('reshape2')

library(reshape2)
library(ggplot2)
rm(list = ls())


task1 <- c('Collect data', '2017-04-01', '2018-04-01')
task2 <- c('Clean data', '2018-04-01', '2018-06-01')
task3 <- c('Analyse data', '2018-06-01', '2019-04-01')
task4 <- c('Write report', '2019-04-01', '2020-04-01')


# df <- as.data.frame(t(sapply(ls(pattern = '^task\\d'), function(x) eval(parse(text = x)))), row.names = FALSE)

df <- as.data.frame(rbind(task1, task2, task3, task4))
names(df) <- c('task', 'start', 'end')
df$task <- factor(df$task, levels = df$task)
df$start <- as.Date(df$start)
df$end <- as.Date(df$end)
df_melted <- melt(df, measure.vars = c('start', 'end'))


# starting date to begin plot
start_date <- as.Date('2017-01-01')

ggplot(df_melted, aes(value, task)) + 
  geom_line(size = 10) +
  labs(x = '', y = '', title = 'Gantt chart using ggplot2') +
  theme_bw(base_size = 20) +
  theme(plot.title = element_text(hjust = 0.5),
        panel.grid.major.x = element_line(colour="black", linetype = "dashed"),
        panel.grid.major = element_blank(),
        panel.grid.minor = element_blank(),
        axis.text.x = element_text(angle = 0)) +
  scale_x_date(date_labels = "%Y %b", limits = c(start_date, NA), date_breaks = '1 year')
