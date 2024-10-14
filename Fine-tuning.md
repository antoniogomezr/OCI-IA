# Tareas de Fine Tuning

- How to Fine-Tune Llama 3 for Specific Use Cases: A Step-by-Step Guide
https://medium.com/@samarrana407/how-to-fine-tune-llama-3-for-specific-use-cases-a-step-by-step-guide-216352e65e0a
- Comparing Top LLM Models: BERT, MPT, Hugging Face & More
https://www.index.dev/blog/comparing-top-llm-models-bert-mpt-hugging-face-and-more

https://medium.com/@srinipotnuru/learn-how-to-fine-tune-a-llama-3-1-with-your-own-custom-data-ead7be969518

Nacho Martinez
Learn about Training Large Language Models in Oracle Cloud Infrastructure
https://www.youtube.com/watch?v=Zj4GGbrEekQ

## Blog de fine tuning
Generating documents through LLMs: A hands-on guide to model fine-tuning
https://medium.com/@arunabh223/generating-documents-through-llms-a-hands-on-guide-to-model-fine-tuning-5e3031b72ce2

A step-by-step guide to fine-tuning large language models for document generation, with a focus on generating standard operating procedures (SOPs)

For those new to fine tuning, it is a process of taking an existing LLM and tweaking it for a particular use case (by training at least one of its parameters). If you are familiar with AI concepts, this is an example of Transfer Learning i.e. using some part of an existing model to develop another model.

### Choose the model

Although there are free models out there which are available for fine-tuning, I will be using the GPT-4o-mini model from OpenAI. The main reason for this is: fine tuning large language models is expensive (either financially or computationally) no matter which approach you choose.

If you try to fine tune an open-source model (like Llama, BERT etc.) , you would need a PC having a powerful GPU or you would need to rent one from a cloud service provider like Azure or AWS.

**Google Colab** is a good option for model fine tuning. Its an cloud based notebook where you can choose a GPU when running your program. 

### Use Case
Writing an SOP for every task is painstaking work. So we are trying to figure out a solution through which we can generate them by entering a simple prompt.
The generated SOP should contain the relevant content and it has to be in a format which complies with our regulatory guidelines.

### Why to fine tune?
why I am using fine tuning for this use case instead of something simpler like prompt engineering.

In my case I am using fine tuning because:
1.- I want to have lower inference costs (i.e. shorter prompts means fewer tokens, faster responses and lower costs). **I don’t want to have to craft detailed prompts** to get the desired output
2.- **I want to set the style and format of the response** . In other words, I want a structured response over a creative response.

1.- The data. Preparing the dataset
**We should ensure that our dataset has a diverse set of examples which would be similar to the conversations we would be having with our model in the future.**

OpenAI recommends the dataset to be in the following format (a jsonl format).

1.a.- Converting the text from the web into a json file.
1.b.- Flattening the json file into a jsonl file by adding \n\n for new sections and \n for new lines.

{"messages": 
   [{"role": "system", 
   "content": "You are a Quality analyst who writes Standard Operating Procedures (SOPs)."}, 
   {"role": "user", 
   "content": "Write an SOP for Cleaning of Tablet Compression Machine."}, 
   {"role": "assistant", 
   "content": "Title: Standard Cleaning Procedure for Tablet Compression Machine\n\n
   1.0 Objective: To lay down the cleaning procedure for the Tablet Compression Machine.\n\n
   2.0 Scope: This SOP shall be applicable for the Tablet Compression Machine used in the compression area of the Production department.\n\n
   3.0 Responsibility:\n3.1 Execution: Operator\n3.2 Checking: Pharmacist and above\n\n
   4.0 Accountability: HOD - Production\n\n5.0 Procedure:\n
   5.1 Put 'OFF' the electric supply before the cleaning operation and disconnect the supply cable.\n
   5.2 Remove the guard, hopper, feed frame, punches, and dies. Wipe off all the surplus powder remaining on the turret. Remove the powder with the help of a vacuum cleaner. Wipe the oil/grease used for lubrication of cam tracks, punches, dies, and pressure rollers with a cotton cloth. Clean the other parts with nylon brushes if required.\n5.3 Clean the hopper, feed frame, and accessories with 25 liters of potable water, and finally rinse with 25 liters of purified water.\n
   5.4 Clean all other parts mentioned above with a 70% v/v solution of Isopropyl Alcohol (IPA).\n
   5.5 Wipe all the parts with a clean, lint-free dry cloth.\n
   5.6 The cleaned machine should be adequately covered.\n
   5.7 All dies and punches after cleaning are to be smeared with food-grade oil.\n
   5.8 Fix the 'Cleaned' label and fill the 'Equipment Log Book.'\n5.9 Re-clean if not used within 72 hours.\n\n
   6.0 Abbreviations:\nSCP: Standard Cleaning Procedure\nIPA: Isopropyl Alcohol"}
   ]
}


2.- Running the fine tuning job
We will fine tuning the GPT-4o-mini model. There are two ways to do this

2.a.-    Using the Fine Tuning UI and uploading our dataset directly.
2.b.-    Writing a python script to create a fine tuning job through the OpenAI API.

When the fine tuning job is finished, you will see a result similar to this.
# output of the fine tuning job
FineTuningJob(id='ftjob-BOQbaQVfqp9PeA8UZqCCItTX', created_at=1728178335, error=Error(code=None, message=None, param=None), fine_tuned_model='ft:gpt-4o-mini-2024-07-18:personal:soptest1:AFAVgk1j', finished_at=1728178682, hyperparameters=Hyperparameters(n_epochs=3, batch_size=1, learning_rate_multiplier=1.8), model='gpt-4o-mini-2024-07-18', object='fine_tuning.job', organization_id='org-ZwARnhihzt89UwpNhgmloWQ5', result_files=['file-gbqjyVhhNsU4zs6SZ4QGsquk'], seed=70067702, status='succeeded', trained_tokens=83427, training_file='file-AK0IH94laEpjfK1ozHLMm6my', validation_file=None, estimated_finish=None, integrations=[], user_provided_suffix='soptest1')

3.- Testing the fine tuned model

**3.1.-Testing with a prompt using the OpenAI playground.**
**3.b.-Testing via a python script.**
