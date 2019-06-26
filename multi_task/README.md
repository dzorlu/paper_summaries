

https://arxiv.org/abs/1905.07553

Given a set of tasks,T , and a computational budget b (e.g., maximum allowable
inference time), what is the optimal way to assign tasks to
networks with combined cost ≤ b such that a combined
measure of task performance is maximized?

The inclusion of an additional task in a network can potentially
improve the accuracy that can be achieved on the existing
tasks, even though the performance of the added task might
be poor. This can be viewed as regularizing or guiding the
loss of one task by adding an additional loss.

 To
that end, we’ve trained networks for all 2
|T | − 1 possible
groupings:
|T |
1

networks with one task,
|T |
2

networks
with two tasks,
|T |
3

networks with three tasks, etc. For the
five tasks we use in our experiments, this is 31 networks, of
which five are single-task networ

MTL
https://arxiv.org/pdf/1901.11504.pdf
Split datasets into groups of tasks

[mt-dnn_1.jpg]

https://arxiv.org/abs/1904.09482
Knowledge distillation is a process of distilling or transferring the knowledge from a (set of)
large, cumbersome model(s) to a lighter, easierto-deploy single model, without significant loss in
performance


First, we pick a few tasks where there are task-specific labeled training data.
Then, for each task, we train an ensemble of different neural nets as a teacher. Each neural net is
an instance of MT-DNN described above, and is fine-tuned using task-specific training data
while the parameters of its shared layers are initialized using the MT-DNN model pre-trained on
the GLUE dataset via MTL, as in Algorithm 1, and the parameters of its task-specific output layers are
randomly initialized.

For each task, the teacher produces the soft targets by averaging the class probabilities across networks. The single
MT-DNN (student) trained to generalize in the same way as the teachers is expected to do much better on test data
than the vanilla MT-DNN that is trained in the normal way on the same training dataset. if task t has a teacher,
the task-specific loss in Line 3 is the average of two objective functions, one for the correct targets and the other
for the soft targets assigned by the teacher.

To obtain a set of diverse single models to form ensemble models (teachers), we first trained 6 single MT-DNNs, initialized using Cased/Uncased
BERT models as (Hancock et al., 2019) with a different dropout rate, ranged in {0.1, 0.2, 0.3}, on the shared layers,
while keeping other training hyperparameters the same as aforementioned. Then, we selected top 3 best models according
to the results on the MNLI and RTE development datasets. Finally, we fine-tuned the 3 models on each of the
MNLI, QQP, RTE and QNLI tasks to form four task-specific ensembles (teachers), each consisting of 3 single MT-DNNs
fine-tuned for the task. The teachers are used to generate soft targets for
the four tasks as Equation 3, described in Section 3. We only pick four out of nine GLUE tasks
to train teachers to investigate the generalization ability of MT-DNNKD, i.e., its performance on the tasks with and
 without teachers.

 First, MT-DNNKD significantly
outperforms MT-DNN and BERTLARGE across
multiple GLUE tasks on the dev sets, which is
consistent with what we observe on test sets in
Table 2. Second, comparing MT-DNNKD with
MT-DNN-ensemble, we see that the MT-DNNKD
successfully distills knowledge from the teachers.
Although the distilled model is simpler than the
teachers, it retains nearly all of the improvement
that is achieved by the ensemble models. More interestingly, we find that incorporating knowledge
distillation into MTL improves the model performance on the tasks where no teacher is used. On
MRPC, CoLA, and STS-B, the performance of
MT-DNNKD is much better than MT-DNN and is
close to the ensemble models although the latter
are not used as teachers in MTL.
5 Conclusion
In this work, we have extended knowledge distillation to MTL in training a MT-DNN for natural language understanding. We have shown that distillation works very well for transferring knowledge
from a set of ensemble models (teachers) into a
single, distilled MT-DNN (student).

at the distilled MT-DNN retains
nearly all of the improvements achieved by ensemble models, while keeping the model size the same
as the vanilla MT-DNN model.

modeling-task-relationships
---------------------------
https://www.kdd.org/kdd2018/accepted-papers/view/modeling-task-relationships-in-multi-task-learning-with-multi-gate-mixture-

the gating networks for dierent tasks can learn different mixture patterns of experts assembling, and thus
capture the task relationships. For each input example, the model is able to select only a
subset of experts by the gating network conditioned on the input.