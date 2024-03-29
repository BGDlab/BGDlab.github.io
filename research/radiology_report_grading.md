SLIP Radiology Report Grading Guidelines and Process

## Overall Purpose

This page discusses the first step in the SLIP duration project. It begins with manual inspection of signed radiology reports from radiologists at CHOP. 

For each report, 
- Read the histories and findings in each note
- In general, be pretty liberal when rating notes
- Use the context, your best judgement, and the guidelines linked in the notebook to assign one of the following grades to each report.
    - Rating 0: Suspect serious imaging pathology
    - Rating 1: Neither a 0 nor a 2 (AKA “I don’t know”). These are people who may or may not be included in our analyses depending on the context.
    - Rating 2: No reason to suspect imaging pathology. These are people whose imaging pathology would likely not exclude them from being part of a control cohort in research brain MRI study. Note that this is different from their PAST MEDICAL HISTORY potentially excluding them, which is a different issue.


## Directions

1. Request access to Arcus Lab 605 and access it from [arcus.chop.edu](arcus.chop.edu). 
2. When access has been granted, turn the lab "ON" and open the Terminal application.
3. Manually type the following command and hit enter to copy the report grading code to your home directory: `cp -r ~/arcus/shared/annotation-helper-tools .`. Copying this code to your home directory makes your copy of it easy to find and work with.
4. Type `ls` and hit enter. You should see a list of directories, one of which will be `annotation-helper-tool`.
5. Now use the menu bar (icon with three horizontal lines located in the top left corner of the webpage) to switch from Terminal to Jupyter.
6. In the Jupyter interface, there is a column on the left side of the page indicating the current directory. Double click on the `annotation-helper-tool` to enter the directory. Double click the file **`Welcome_To_SLIP_Radiology_Report_Grading.ipynb`** file to open it in the Jupyter file editor.
7. Follow the instructions in the **`Welcome_To_SLIP_Radiology_Report_Grading.ipynb`** notebook and use the linked document as a guideline for grading the reports. When you have finished the **`Welcome_To_SLIP_Radiology_Report_Grading.ipynb`** notebook, switch to the **`Arcus_Radiology_Report_Grading.ipynb`** notebook to grade new reports.

## Refreshing your copy of the code

As the process evolves, the code for grading the reports will be updated on Arcus. When you are asked to get an updated version of the code for yourself, follow these steps:

1. Log in to [arcus.chop.edu](arcus.chop.edu) and open lab 605. 
2. Turn the lab ON and open the Terminal application.
3. Type the following commands and hit Enter to remove the existing code (not data, just code): `cd ~` (followed by Enter) and `rm -rf annotation-helper-tools`
4. Then type the following command and hit enter to copy the report grading code to your home directory: `cp -r ~/arcus/shared/annotation-helper-tools .`.
5. When you open the Jupyter notebook interface, you will need to modify the `name` variable and the `project` variable.

Note: if you perform this refresh after you have been working in an earlier version of the code in Jupyter, you will need to restart the kernel (Kernel -> Restart Kernel).

## Examining the reliability ratings

Make sure you have pulled or refreshed your code (previous 2 sections) after 12 pm Thursday June 22, 2023.

1. Log in to [arcus.chop.edu](arcus.chop.edu). 
2. Navigate to "Applications" -> "My Labs" and open lab 605.
3. Turn the lab ON and open the Jupyter notebook application.
4. In the Jupyter interface, there is a column on the left side of the page indicating the current directory. Double click on the `annotation-helper-tool` to enter the directory. Double click the file **`Radiology_Report_Grading_status.ipynb`** file to open it in the Jupyter file editor.
5. Follow the instructions at the top of the file.

If you would like to view the reliability report comparisons for grader pairs other than the ones listed, follow the instructions to add graders to Cells 04-07. If the desired grader's name is not printed in the output of Cell 03, then they have not done the reliability ratings yet.


## Help

For help, reach out to Jenna via Slack. 
