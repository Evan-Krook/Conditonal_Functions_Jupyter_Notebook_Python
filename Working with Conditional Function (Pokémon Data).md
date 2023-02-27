```python
import project
import math
```

Rules for Pokémon battles:
Now, here are the rules governing Pokémon battles:

1. A Pokémon battle takes place between two Pokémon
2. The two Pokémon take turns attacking each other.
3. The Pokémon with the higher Speed stat attacks first.
4. On each turn, the attacking Pokémon can choose between two modes of attack - Physical or Special.
5. In addition to the attack mode, each Pokémon can choose the type of its attack.
6. Based on the move chosen by the attacking Pokémon, the defending Pokémon receives damage to its HP.
7. If a Pokémon's HP drops to (or below) 0, it faints and therefore loses the battle.
8. However, if one Pokémon is much stronger than the other, then the weaker Pokémon will run away instead of fighting.


Throughout this project, we will break this down into smaller parts and slowly build up to the battle function. Eventually the battle function will determine the outcome of a battle between any two Pokémon.


The first thing we need to do is calculate the damage caused by one Pokémon's attack on another Pokémon. To accomplish this, we need to create the function damage

**Damage Function**


```python
def damage(attacker, defender):
    physical_damage = 10 * project.get_attack(attacker) / project.get_defense(defender)
    special_damage = 10 *  project.get_sp_atk(attacker) / project.get_sp_def(defender)
    if physical_damage > special_damage:
        return physical_damage
    else:
        special_damage > physical_damage
        return special_damage
```

**Question 1:** How much damage does `Feraligatr` do to `Aipom`?


```python
damage_feraligatr_aipom = damage('Feraligatr', 'Aipom')

damage_feraligatr_aipom
```




    19.09090909090909



**Type of Attack Bouns**


```python
def type_bonus(attack_type, defender):
    defender_type1 = project.get_type1(defender)
    defender_type2 = project.get_type2(defender)
    if defender_type2 == 'DNE':
        bonus = project.get_type_effectiveness(attack_type, defender_type1)
        return bonus
    else:
        bonus = project.get_type_effectiveness(attack_type, defender_type1) * project.get_type_effectiveness(attack_type, defender_type2)
        return bonus
```

**Question 2**: How effective is Poison type against Rayquaza?


```python
bonus_poison_rayquaza = type_bonus('Poison', 'Rayquaza')

bonus_poison_rayquaza
```




    1.0



**Effective Damage Function**


```python
def get_num_types(attacker):
    if project.get_type1(attacker) == 'DNE':
        return 0
    elif project.get_type2(attacker) == 'DNE':
        return 1
    else: 
        return 2
```


```python
def effective_damage(attacker, defender):
    num_type = get_num_types(attacker)
    bonus_155 = type_bonus(project.get_type1(attacker), defender)
    damage_done = damage(attacker, defender)
    if num_type == 1 :
        return (damage_done * bonus_155)
    elif num_type == 2 :
        bonus_166 = type_bonus(project.get_type2(attacker), defender)
        return (damage_done * max(bonus_155, bonus_166))
    else:
        return 'No Output'
```

**Question 3**: How much damage does Pikachu do to Weedle?


```python
eff_damage_pikachu_weedle = effective_damage('Pikachu', 'Weedle')

eff_damage_pikachu_weedle
```




    25.0



**How Many Hit Before Fainting Function**


```python
def num_hits(attacker, defender):
    return math.ceil(project.get_hp(defender)/effective_damage(attacker, defender))
```

**Question 4**: How many hits can the defending Pokémon Garchomp take from Golem(attacker)?


```python
hits_garchomp_golem = num_hits('Golem', 'Garchomp')

hits_garchomp_golem
```




    9



**Battle Simulator**


```python
def battle(pkmn1, pkmn2):
    if num_hits(pkmn2, pkmn1) - num_hits(pkmn1,pkmn2) > 10:
        return str(pkmn2) + " " + 'ran away' 
    elif num_hits(pkmn1,pkmn2) - num_hits(pkmn2, pkmn1) > 10:
        return str(pkmn1) + " " + 'ran away' 
    elif num_hits(pkmn1,pkmn2) > num_hits(pkmn2, pkmn1):
        return pkmn2
    elif num_hits(pkmn1,pkmn2) < num_hits(pkmn2, pkmn1):
        return pkmn1
    elif num_hits(pkmn1,pkmn2) == num_hits(pkmn2, pkmn1):
        if project.get_speed(pkmn1) > project.get_speed(pkmn2):
            return pkmn1
        elif project.get_speed(pkmn1) > project.get_speed(pkmn2):
            return pkmn2
        else:
            return "Draw"
```

**Question 5**: What is the output of battle('Haunter', 'Rhydon')?


```python
battle_haunter_rhydon = battle('Haunter', 'Rhydon')

battle_haunter_rhydon
```




    'Rhydon'



**Question 6**: What is the output of battle('Espeon', 'Raichu')??


```python
battle_espeon_raichu = battle('Espeon', 'Raichu')

battle_espeon_raichu
```




    'Espeon'



**Question 7**: What is the output of battle('Onix', 'Hydreigon')?


```python
battle_onix_hydreigon = battle('Onix', 'Hydreigon')

battle_onix_hydreigon
```




    'Onix ran away'



**Friendship Potentinal Function**


```python
def get_stat_total(pkmn):
    total_stat= (project.get_hp(pkmn) + project.get_attack(pkmn) + project.get_defense(pkmn) + project.get_sp_atk(pkmn)+ 
    project.get_sp_def(pkmn)+ project.get_speed(pkmn))
    return total_stat

def friendship_score(pkmn1, pkmn2):
    result = []
    if project.get_region(pkmn1) == project.get_region(pkmn2):
        solu= 1
        result.append(solu)
    if abs(get_stat_total(pkmn1)-get_stat_total(pkmn2)) <= 20:
        solu= 1
        result.append(solu)
    if project.get_type1(pkmn1) == project.get_type1(pkmn2):
        solu= 1
        result.append(solu)
    if project.get_type2(pkmn1) != 'DNE' and project.get_type2(pkmn2) != 'DNE':
        if project.get_type2(pkmn1) == project.get_type2(pkmn2):
            solu= 1
            result.append(solu)
        if project.get_type1(pkmn1) != project.get_type2(pkmn2):
            if project.get_type2(pkmn2) != project.get_type1(pkmn2):
                if project.get_type2(pkmn1) != 'DNE' and project.get_type2(pkmn2) != 'DNE':
                    if project.get_type1(pkmn1) == project.get_type1(pkmn2) and project.get_type2(pkmn1) == project.get_type2(pkmn2):
                        solu= 1
                        result.append(solu)
    else:
        result.append(0)
    return sum(result)
```

**Question 8**: What is the output of friendship_score('Aipom', 'Zekrom')?


```python
friendship_aipom_zekrom = friendship_score('Aipom', 'Zekrom')

friendship_aipom_zekrom
```




    0



**Question 9**: What is the output of friendship_score('Weedle', 'Kakuna')?


```python
friendship_weedle_kakuna = friendship_score('Weedle', 'Kakuna')

friendship_weedle_kakuna
```




    5


