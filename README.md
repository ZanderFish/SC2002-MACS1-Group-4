# Restaurant Rush

A turn-based restaurant management game for the SC2002 group project. The game
is written in Java and runs in a command-line interface. The repository keeps
three runnable milestones as Git tags:

| Stage | Features | Runnable snapshot |
|---|---|---|
| [Stage 1](https://github.com/ZanderFish/SC2002-MACS1-Group-4/tree/stage-1) | Baseline game with regular customers, menu, staff, turns, orders, payment, and win/lose conditions | [`stage-1`](https://github.com/ZanderFish/SC2002-MACS1-Group-4/tree/stage-1) |
| [Stage 2](https://github.com/ZanderFish/SC2002-MACS1-Group-4/tree/stage-2) | Adds VIP and Critic customers | [`stage-2`](https://github.com/ZanderFish/SC2002-MACS1-Group-4/tree/stage-2) |
| [Stage 3](https://github.com/ZanderFish/SC2002-MACS1-Group-4/tree/stage-3) | Adds the Host, ComboMeal, and Happy Hour | [`stage-3`](https://github.com/ZanderFish/SC2002-MACS1-Group-4/tree/stage-3) |

Repository: <https://github.com/ZanderFish/SC2002-MACS1-Group-4>

## Build and run

Install a JDK that provides `javac` and `java`, then run the commands from the
repository root. Compilation output is written to `out/`.

**Windows (Command Prompt):**

```bat
run.bat
```

**macOS/Linux:**

```sh
./run.sh
```

To run a particular milestone, check out its tag first (for example,
`git checkout stage-2`) and use the same command. Return to your branch with
`git switch <branch-name>` when finished.

## Reproducible runs

Each milestone includes scripted victory and defeat runs under `tests/`. The
input files include comments describing the expected choices and outcomes;
`--echo` prints accepted inputs alongside the game output.

**Windows (Command Prompt):**

```bat
run.bat --echo < tests\victory.txt
run.bat --echo < tests\defeat.txt
```

**macOS/Linux:**

```sh
./run.sh --echo < tests/victory.txt
./run.sh --echo < tests/defeat.txt
```

Run these commands after checking out each stage tag to reproduce that stage's
scenarios. The Stage 1 and Stage 2 victory scripts also exercise an invalid
action followed by a valid retry. Stage 2's victory includes both a Critic and
a VIP; Stage 3's victory includes a Critic, a VIP, combos, and Happy Hour.
Defeat scripts leave the waiting customers unserved to demonstrate the loss
outcome.

## Design documentation

- [Stage 1 class diagram](docs/Stage_1_Class_Diagram.png)
- [Stage 2 class diagram](docs/Stage_2_Class_Diagram.png)
- [Stage 3 class diagram](docs/Stage_3_Class_Diagram.png)
- [Stage 3 end-to-end sequence diagram](docs/UML_Sequence_Diagram.png)
- [VIP customer behavior sequence diagram](docs/VIPCustomer_Sequence_Diagram.png)

## Stage evolution

Compare links show the exact changes between the tagged snapshots:

- [Stage 1 to Stage 2](https://github.com/ZanderFish/SC2002-MACS1-Group-4/compare/stage-1...stage-2)
- [Stage 2 to Stage 3](https://github.com/ZanderFish/SC2002-MACS1-Group-4/compare/stage-2...stage-3)

### Stage 1 to Stage 2

| Change | Files | Reason |
|---|---|---|
| Added | `src/restaurantrush/customer/VIPCustomer.java`, `src/restaurantrush/customer/CriticCustomer.java` | Add customer-specific satisfaction loss, ordering behavior, and VIP billing behavior through customer subclasses. |
| Added | `docs/Stage_1_Class_Diagram.png`, `docs/Stage_2_Class_Diagram.png` | Record the design at the first two milestones. |
| Modified | `src/restaurantrush/boundary/ConsoleUI.java`, `src/restaurantrush/control/GameConfig.java` | Display and configure the new customer types and the Stage 2 arrival schedule. |
| Modified | `src/restaurantrush/entity/Money.java` | Support the VIP percentage discount using integer-cent money calculations. |
| Modified | `tests/victory.txt`, `tests/defeat.txt` | Provide Stage 2 reproducible runs, including Critic/VIP behavior in the victory scenario and a defeat scenario. |

**Earlier behavior retained/checks:** The Stage 2 victory and defeat scripts
continue to cover the 18-turn game outcomes. The victory scenario also exercises
the baseline order-to-payment flow, staff-task constraints, and invalid-action
retry while adding Critic ordering and VIP pricing; the defeat scenario checks
that leaving customers unserved still results in defeat.

### Stage 2 to Stage 3

| Change | Files | Reason |
|---|---|---|
| Added | `src/restaurantrush/entity/ComboMeal.java`, `src/restaurantrush/entity/HappyHour.java`, `src/restaurantrush/staff/Host.java` | Implement combo pricing/preparation, the temporary Happy Hour policy, and the additional seating role. |
| Added | `docs/Stage_3_Class_Diagram.png` | Record the final-stage design. |
| Modified | `src/restaurantrush/boundary/ConsoleUI.java` | Add manager choices and display for the Host and Happy Hour, and show combo items. |
| Modified | `src/restaurantrush/control/GameConfig.java`, `src/restaurantrush/control/GameController.java` | Configure Stage 3 menu/arrivals and coordinate the new role and turn-based Happy Hour behavior. |
| Modified | `src/restaurantrush/customer/Customer.java` | Add satisfaction recovery for customers waiting for service during Happy Hour. |
| Modified | `src/restaurantrush/staff/Waiter.java` | Apply Happy Hour pricing when an order is placed and support combo menu items. |
| Modified | `tests/victory.txt`, `tests/defeat.txt` | Provide reproducible Stage 3 runs that include the new features and preserve the win/lose scenarios. |

**Earlier behavior retained/checks:** The Stage 3 victory script still includes
Critic and VIP customers, order preparation and serving, eating, payment, and a
final result check while adding Host seating, combos, and Happy Hour. It
demonstrates that the VIP discount is applied before the Happy Hour discount.
The defeat script keeps the no-service loss case and now also covers the Host
choice.

## Video

[Watch the recorded presentation on YouTube](https://youtu.be/r1FSg33gsho)

### Video timestamps

| # | Segment | Timestamp |
|---|---|---|
| 1 | **Akshith:** Launch the submitted game and introduce the goal and staff roles. Demonstrate C1 progressing from READY on Turn 1 to SERVED on Turn 2 and payment on Turn 4, including eating and automatic cashier payment. Introduce the Critic’s arrival. | **0:00** |
| 2 | **Zhitong:** Cover Turns 7–12 of the victory replay. Demonstrate the VIP’s patience and discount, activate Happy Hour, and show the Critic’s combo being served and paid for. | **2:35** |
| 3 | **Shi Jye:** Cover Turns 13–18 of the victory replay. Explain starting C5’s long combo early, show both tables occupied at Turn 16, then demonstrate a table becoming available, the next customer being seated, and the final victory result. | **6:32** |
| 4 | **Saamya:** Run the defeat scenario using `defeat.txt`. Show customers waiting, losing satisfaction, leaving without paying, and the final defeat result. | **9:23** |
| 5 | **Zander:** Present the Stage 1, Stage 2, and Stage 3 class diagrams in order. Explain the added customer types in Stage 2, Stage 3 features such as ComboMeal and HappyHour, and how the classes work together. | **11:17** |
