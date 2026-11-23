---
title: "formal/examples/ReadExample.lean"
noindex: true
---

```lean
import MonadFormal.NoGo
import MonadFormal.Quotient

open MonadFormal

namespace ReadExample

abbrev PartState := Bool × Bool

def availability (x : PartState) : Bool := x.1
def repair (x : PartState) : Bool := x.1 && x.2

def repairFamily : ProbeFamily PartState where
  I := Unit
  O := fun _ ↦ Bool
  f := fun _ ↦ repair

#eval availability (true, false)
#eval availability (true, true)
#eval repair (true, false)
#eval repair (true, true)

#check @Factors
#check @Sufficient
#check @toQuot_sufficient
#check @quotient_universal
#check @no_go

end ReadExample
```
