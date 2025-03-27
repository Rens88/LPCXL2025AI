# LPCXL2025AI
Example code for the 2025 LPC XL.

With "fake_injury_data_generation" a fake dataset (with or without bias) can be generated.
The "Data Description" below describes in detail the content of the data.
It can be used to start prompting an AI chatbot.

## Data Description
Dataframe "df" contains information about athletes (denoted by "user_id") over time (denoted by "date") during a 2 year period.
It contains a column "injury_onset" (Boolean) that indicates whether that athlete got injured on that day.
The column "to_be_injured" contains either 0s, 1s or NA (int64) and can be used for (logistic) regression.
The column "currently_injured" (Boolean) denotes whether the athlete is (still) injured on that day.
Each injury period is denoted by a unique identifier ("injury_idx", string).
There is also a helper column "days_until_next_injury" that denotes the number of days until the next injury.
And there is a helper column "next_injury_idx" that indicates what the injury_idx is of that upcoming injury.
There is a helper column "days_since_prev_injury_end" indicating the number of days since the previous injury.
There are also three Boolean columns that indicate an upcoming injury: upcoming_injury_within_28_days, upcoming_injury_within_14_days, upcoming_injury_within_07_days.

Each practice (max 1 per day) has an "rpe" (1-10) and a duration ("duration_minutes").
The distribution of the rpes is characterized by "training_distribution" (pyramidal or polarized).

Each practice day is also characterized by "session_rpe" (rpe * duration_minutes).
Each day is also characterized by:
     session_rpe_SUM_07_days: The sum of session_rpe in the past 7 days.
     session_rpe_STD0_07_days: The standard deviation of session_rpe in the past 7 days where days without practice have been assigned a 0.
     session_rpe_SUM_28_days: The sum of session_rpe in the past 28 days.
     session_rpe_STD0_28_days: The standard deviation of session_rpe in the past 28 days where days without practice have been assigned a 0.
     session_rpe_AC07_28_days: The acute (past 7 days) chronic (past 28 days) workload ratio. Values below 1 denote relative undertraining, above 1 overtraining.
     session_rpe_MON_28_days: the monotony of the past 28 days
     session_rpe_STR_28_days: the strain of the past 28 days

There are some metrics (1-10) that quantify the athlete's wellbeing: "Energy Score",	"Mood Score", "Recovery Score", "Motivation Score", "Confidence Score", "Sleep Quality Score".'
These metrics have been summarized for each day by taking the average ("Wellness Average").

Most importantly, it contains a set of columns with the suffix "_AVG_prev_07_days" that summarize the 7 days preceding the current day.
Three temporal aggregation methods were applied to summarize the 7 days preceding an injury: average, daily average and slope.
These columns should be used in combination for any predictions of "to_be_injured".

## Example questions
- I'm a coach and I want to improve my training schedules. Suggest me some analyses with the described data.
- I'm a scientist and I want to examine whether "seilers blackhole" exists (increased risk with too many minutes with rpe 5 and 6). Create the code to examine this research question.
- I'm a technical director and I want to know where the sport can be improved the most. Suggest me an analysis.
- I'm a presenter showcasing the power of prompting. Suggest me some more example questions that I can show the