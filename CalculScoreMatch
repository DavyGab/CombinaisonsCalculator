<?php

Class CalculScoreMatch {
    const TEAM_1 = 1;
    const TEAM_2 = 2;

    const EXCEEDED_SCORE = 1;
    const END_MATCH = 2;

    private $results = [];

    private $maxScoreEquipe = [];

    private $possiblePoints = [1, 2];


    public function __construct ($scoreTeam_1, $scoreTeam_2)
    {
        $this->maxScoreEquipe[self::TEAM_1] = intval($scoreTeam_1);
        $this->maxScoreEquipe[self::TEAM_2] = intval($scoreTeam_2);
    }

    public static function showScore($score)
    {
        $return = '---';

        $i = 1;
        foreach ($score as $s) {
            $return .= ($s > 0 ? "\033[0;31m " : ' ') . abs($s);

            if ($i !== count($score)) {
                $return .= ' -';
            }
            $return .= ($s > 0 ? "\033[0m" : '');

            $i++;
        }
        $return .= ' ---';

        echo $return . "\r\n";
        return $return;
    }

    public function findNextScoreRecursively($partialScore = [])
    {
        if (self::END_MATCH === $this->getMatchState($partialScore)) {
//            $this->results[] = $partialScore;
            self::showScore($partialScore);
            return;
        }
        foreach ($this->possiblePoints as $p) {
            $partialScore[] = $p;

            if (self::EXCEEDED_SCORE !== $this->getMatchState($partialScore)) {
                $this->findNextScoreRecursively($partialScore);
            }
            array_pop($partialScore);
        }
    }

    private function sumBothTeam($arrayScore)
    {
        $tempScore = array(
            self::TEAM_1 => 0,
            self::TEAM_2 => 0
        );

        foreach ($arrayScore as $score) {
            if ($score > 0) {
                $tempScore[self::TEAM_1] += $score;
            } else {
                $tempScore[self::TEAM_2] += -1*$score;
            }
        }
        return $tempScore;
    }

    private function getMatchState($score)
    {
        $sum = $this->sumBothTeam($score);
        if ($sum[self::TEAM_1] == $this->maxScoreEquipe[self::TEAM_1] && $sum[self::TEAM_2] == $this->maxScoreEquipe[self::TEAM_2]) {
            return self::END_MATCH;
        }

        if ($sum[self::TEAM_1] > $this->maxScoreEquipe[self::TEAM_1] || $sum[self::TEAM_2] > $this->maxScoreEquipe[self::TEAM_2]) {
            return self::EXCEEDED_SCORE;
        }

        return null;
    }
    public function showAllScores()
    {
        foreach ($this->results as $r) {
            echo $this->showScore($r) . "\r\n";
        }
    }
}

$classScore = new CalculScoreMatch($argv[1], $argv[2] = 0);
$classScore->findNextScoreRecursively();
//$classScore->showAllScores();
