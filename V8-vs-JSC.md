# V8 vs. JSC
## V8
```
IGNITION_HANDLER(JumpIfTrue, InterpreterAssembler) {
  Node* accumulator = GetAccumulator();
  Node* relative_jump = BytecodeOperandUImmWord(0);
  Node* true_value = BooleanConstant(true);
  CSA_ASSERT(this, TaggedIsNotSmi(accumulator));
  CSA_ASSERT(this, IsBoolean(accumulator));
  JumpIfWordEqual(accumulator, true_value, relative_jump);
}
```

```
void InterpreterAssembler::JumpIfWordEqual(Node* lhs, Node* rhs, Node* delta) {
  JumpConditional(WordEqual(lhs, rhs), delta);
}
```

```
void InterpreterAssembler::JumpConditional(Node* condition, Node* delta) {
  Label match(this), no_match(this);
  Branch(condition, &match, &no_match);
  BIND(&match);
  Jump(delta);
  BIND(&no_match);
  Dispatch();
}
```

```
void RawMachineAssembler::Branch(Node* condition, RawMachineLabel* true_val,RawMachineLabel* false_val) {
  DCHECK(current_block_ != schedule()->end());
  Node* branch = MakeNode(common()->Branch(), 1, &condition);
  schedule()->AddBranch(CurrentBlock(), branch, Use(true_val), Use(false_val));
  current_block_ = nullptr;
}
```

```
void Schedule::AddBranch(BasicBlock* block, Node* branch, BasicBlock* tblock, BasicBlock* fblock) {
  DCHECK_EQ(BasicBlock::kNone, block->control());
  DCHECK_EQ(IrOpcode::kBranch, branch->opcode());
  block->set_control(BasicBlock::kBranch);
  AddSuccessor(block, tblock);
  AddSuccessor(block, fblock);
  SetControlInput(block, branch);
}
```

## JSC

```
LLINT_SLOW_PATH_DECL(slow_path_jtrue)
{
    LLINT_BEGIN();
    LLINT_BRANCH(op_jtrue, LLINT_OP_C(1).jsValue().toBoolean(exec));
}
```

```
#define LLINT_BRANCH(opcode, condition) do {  \
        bool __b_condition = (condition);\
        LLINT_CHECK_EXCEPTION();\
        if (__b_condition)\
            pc += pc[OPCODE_LENGTH(opcode) - 1].u.operand;\
        else\
            pc += OPCODE_LENGTH(opcode);\
        LLINT_END_IMPL();\
    } while (false)
```

