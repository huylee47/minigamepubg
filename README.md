
(function() {
    const SPEED = 0.2; // 0.5 = chậm 50%, 0.25 = chậm 75%

    // Override requestAnimationFrame
    const originalRAF = window.requestAnimationFrame;
    let lastTime = 0;
    let accumulatedTime = 0;

    window.requestAnimationFrame = function(callback) {
        return originalRAF.call(window, function(timestamp) {
            if (!lastTime) lastTime = timestamp;
            const delta = timestamp - lastTime;
            lastTime = timestamp;
            accumulatedTime += delta * SPEED;
            callback(accumulatedTime);
        });
    };

    // Override performance.now
    const originalPerfNow = performance.now.bind(performance);
    const startReal = originalPerfNow();
    let startFake = startReal;

    performance.now = function() {
        const real = originalPerfNow();
        const elapsed = (real - startReal) * SPEED;
        return startReal + elapsed;
    };


    const originalDateNow = Date.now;
    const startDateReal = originalDateNow();

    Date.now = function() {
        const real = originalDateNow();
        const elapsed = (real - startDateReal) * SPEED;
        return startDateReal + elapsed;
    };

    console.log(`✅ Giảm tốc độ xuống ${SPEED}x`);
})();
