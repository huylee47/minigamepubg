# ⚠️ SAU KHI COPY CODE VÀO SOURCE NHỚ NHẤN `CTRL + S` ĐỂ LƯU FILE ĐÈ ⚠️

        handleGameOver(t) {
        if (this.isGameOver)
            return;
        if (this.allowTrueGameOver) {
            this.isGameOver = !0,
            this.audio.stopAllSfx(),
            this.audio.fadeOutBgm(450),
            this.audio.playSfx("sfx_player_death"),
            this.player.clearInvincibility(),
            this.lowHealthVignette.hide(),
            this.setGamePaused(!1),
            this.redZones.setPaused(!0);
            const e = this.hud.getScore();
            this.player.playDeathAnimation(( () => {
                !function(t) {
                    const e = t.deathDelayMs ?? 2e3
                      , i = async function(t, e) {
                        return vi || (vi = async function(t, e) {
                            const i = Si.getRoundSession();
                            if (i.abnormalExit || i.resultReported || !i.startedAt || !i.roundToken || !i.sessionToken)
                                return null;
                            const s = (new Date).toISOString()
                              , n = function(t) {
                                return {
                                    npcDodged: t.npcDodged,
                                    crateCollected: t.crateCollected,
                                    redzoneSurvived: t.redzoneSurvived,
                                    maxCombo: t.maxCombo,
                                    levelReached: t.levelReached
                                }
                            }(i);
                            try {
                                const r = await async function(t) {
                                    return mi("/v1/public/rounds/submit", {
                                        method: "POST",
                                        body: JSON.stringify(t)
                                    }, 15e3)
                                }({
                                    roundToken: i.roundToken,
                                    sessionToken: i.sessionToken,
                                    attemptIndex: i.attemptIndex,
                                    score: t,
                                    startedAt: i.startedAt,
                                    endedAt: s,
                                    endReason: e,
                                    details: n
                                });
                                i.endReason = e,
                                i.endedAt = s,
                                i.resultReported = !0;
                                const a = await Si.notifyResult({
                                    ...r,
                                    details: n
                                });
                                if (di(a) && r.rankApplied)
                                    try {
                                        await async function(t) {
                                            return mi("/v1/public/rounds/revoke", {
                                                method: "POST",
                                                body: JSON.stringify(t)
                                            }, 1e4)
                                        }({
                                            roundToken: i.roundToken,
                                            sessionToken: i.sessionToken,
                                            attemptIndex: i.attemptIndex,
                                            reason: a.code
                                        })
                                    } catch {}
                                return {
                                    response: r,
                                    hostReply: a
                                }
                            } catch {
                                return null
                            }
                        }(t, e).finally(( () => {
                            vi = null
                        }
                        )),
                        vi)
                    }(t.score, t.endReason).catch((t => (Si.emitError({
                        code: t instanceof gi ? t.code : "SUBMIT_FAILED",
                        message: t instanceof Error ? t.message : hi("error.roundSubmitFailed"),
                        fatal: !1
                    }),
                    null)));
                    t.scene.time.delayedCall(e, ( () => {
                        !async function(t, e) {
                            t.rankingLoadingDialog.show("dialog.rankingSubmitting");
                            const i = await e;
                            t.rankingLoadingDialog.hide(),
                            t.onTransition({
                                score: t.score,
                                submitResponse: (null == i ? void 0 : i.response) ?? null,
                                hostReply: (null == i ? void 0 : i.hostReply) ?? null
                            })
                        }(t, i)
                    }
                    ))
                }({
                    scene: this,
                    score: e,
                    endReason: t,
                    rankingLoadingDialog: this.rankingLoadingDialog,
                    onTransition: ({score: t, submitResponse: e, hostReply: i}) => {
                        this.scene.start("GameOver", {
                            score: t,
                            submitResponse: e,
                            hostReply: i
                        })
                    }
                })
            }
            ));
            return;
        }

        if (!this.immortalTimeoutStarted) {
            this.immortalTimeoutStarted = true;

            // 1. Tạo sẵn một lệnh để bạn gõ từ Console ép game over ngay lập tức
            window.endGame = () => {
                console.log(" Lệnh hủy diệt đã kích hoạt! Game over...");
                this.allowTrueGameOver = true;
                this.handleGameOver(t);
            };
            console.log(" Đã bật bất tử! Khi nào muốn dừng và LƯU ĐIỂM, hãy gõ lệnh: window.endGame()");

            setTimeout(() => {
               if (!this.allowTrueGameOver) window.endGame();
            }, 170 * 1000); 
        } else {
            console.log("Đang trong thời gian bất tử, bỏ qua va chạm này!");
        }
    }
