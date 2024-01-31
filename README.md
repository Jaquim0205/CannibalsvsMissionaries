% Declaramos el estado inicial
initialState(state(3, 3, left, 0, 0)).

% Declaramos el estado objetivo
goalState(state(0, 0, right, 3, 3)).

% Declaramos las transiciones de estado válidas
valid(state(M1, C1, left, M2, C2)) :-
    M1 >= 0, C1 >= 0, M2 >= 0, C2 >= 0,
    M1 >= C1, M2 >= C2.

valid(state(M1, C1, right, M2, C2)) :-
    M1 >= 0, C1 >= 0, M2 >= 0, C2 >= 0,
    M1 >= C1, M2 >= C2.

% Declaramos las transiciones de estado
move(state(M1, C1, left, M2, C2), state(M1New, C1New, right, M2New, C2New)) :-
    between(0, 2, M), between(0, 2, C),
    M1New is M1 - M, C1New is C1 - C,
    M2New is M2 + M, C2New is C2 + C,
    valid(state(M1New, C1New, right, M2New, C2New)).

move(state(M1, C1, right, M2, C2), state(M1New, C1New, left, M2New, C2New)) :-
    between(0, 2, M), between(0, 2, C),
    M1New is M1 + M, C1New is C1 + C,
    M2New is M2 - M, C2New is C2 - C,
    valid(state(M1New, C1New, left, M2New, C2New)).

% ... (Código anterior)

% Declaramos la impresión del estado
printState(state(M1, C1, Boat, M2, C2)) :-
    format('Izquierda: ~d misionarios, ~d caníbales\n', [M1, C1]),
    format('Bote: ~w\n', [Boat]),
    format('Derecha: ~d misionarios, ~d caníbales\n\n', [M2, C2]).

% Declaramos la búsqueda recursiva con impresión
path(Start, Start, _).
path(Start, Goal, Visited) :-
    move(Start, Next),
    \+ member(Next, Visited),
    printState(Next),
    path(Next, Goal, [Next | Visited]).

% ... (Código posterior)

% Consulta para encontrar e imprimir una solución
play :-
    initialState(InitialState),
    goalState(GoalState),
    printState(InitialState),
    path(InitialState, GoalState, [InitialState]).
