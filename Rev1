var BTImportMech = BTImportMech || (function(){
    
    //Keys are attribute variable names in Roll20
    //Values are keys for armorVal structure used to call setArmor
    var armorAttr = {
            "lefttorso_armor"       : "L/R Torso",
            "righttorso_armor"      : "L/R Torso",
            "centertorso_armor"     : "Center Torso",
            "lefttorso_rear_armor"  : "L/R Torso (rear)",
            "righttorso_rear_armor" : "L/R Torso (rear)",
            "centertorso_rear_armor": "Center Torso (rear)",
            "leftarm_armor"         : "L/R Arm",
            "rightarm_armor"        : "L/R Arm",
            "leftleg_armor"         : "L/R Leg",
            "rightleg_armor"        : "L/R Leg",
            "head_armor"            : "Head"
    }
    
    //Keys are attribute variable names in Roll20
    //Values are keys for armorVal structure used to call setArmor
    var structAttr = {
            "lefttorso_internalstructure"   : "L/R Torso",
            "righttorso_internalstructure"  : "L/R Torso",
            "centertorso_internalstructure" : "Center Torso",
            "leftarm_internalstructure"     : "L/R Arm",
            "rightarm_internalstructure"    : "L/R Arm",
            "leftleg_internalstructure"     : "L/R Leg",
            "rightleg_internalstructure"    : "L/R Leg",
            "head_internalstructure"        : "Head"
    }
    
    var MPAttr = [
        "mech_walk",
        "mech_run",
        "mech_sprint",
        "mech_jump"
        ]
        
    var miscAttr = [
        "mech_name",
        "mech_tonnage"
        ]
    
    function setName(charID,name) {
        var mechAttr = findObjs({
            _characterid: charID,
            _type: "attribute",
            name: "mech_name"
        })[0];
        mechAttr.set("current",name);
        return;
    }
    
    function setTonnage(charID,ton){
        var mechAttr = findObjs({
            _characterid: charID,
            _type: "attribute",
            name: "mech_tonnage"
        })[0];
        mechAttr.set("current",ton);
        return;
    }
    
    //Gets armor and internal structure values 
    function getArmor(mechData){
        //Container for all mech armor and internal structure values
        var mechArmor = {};
        
        //Matching all entries of head armor and internal structure
        head = mechData.match(/Head\s*(\d)+\s*(\d)*/g);
        arrlength = head.length;
        for (var i=0; i<arrlength; i++){
            headKey = head[i].match(/Head/g);
            headValue = head[i].match(/(\d)+/g);
            mechArmor[headKey] = headValue;
        } 
        //Matching all entries of torso armor and internal structure
        torso = mechData.match(/(\S* Torso [\(\)a-z]*)\s*(\d)*\s*(\d)*/g);
        arrlength = torso.length;
        for (var i=0; i<arrlength; i++){
            torsoKey = torso[i].match(/\S* Torso( \(rear\))?/g);
            torsoValue = torso[i].match(/(\d)+/g);
            mechArmor[torsoKey] = torsoValue;
        }
        //So far, I've only seen 1 arm entry. Not sure how asymmetrical mechs
        //are formatted. Have made the regex generic in case it does Left  Arm
        // and Right Arm
        arm = mechData.match(/[A-z\/]+ Arm\s*(\d)+\s*(\d)*/g);
        arrlength = arm.length;
        for (var i=0; i<arrlength; i++){
            armKey = arm[i].match(/[A-z\/]+ Arm/g);
            armValue = arm[i].match(/(\d)+/g);
            mechArmor[armKey] = armValue;
        }
        leg = mechData.match(/[A-z\/]+ Leg\s*(\d)+\s*(\d)*/g);
        arrlength = leg.length;
        for (var i=0; i<arrlength; i++){
            legKey = leg[i].match(/[A-z\/]+ Leg/g);
            legValue = leg[i].match(/(\d)+/g);
            mechArmor[legKey] = legValue;
        } 
        
        return mechArmor;
    }
    
    function setArmor(charID, armorVal){
        //Will go through each entry in the armorAttr table and set armor values
        for (attr in armorAttr){
            log(attr);
            log(armorAttr[attr]);
            log(armorVal[armorAttr[attr]])
            //Grabbing the values for this location inside passed armorVal object
            //Currently based on text input in Bio section
            armorStats = armorVal[armorAttr[attr]];
            //Armor is the last value in all entries
            armorIndex = armorStats.length - 1;
            armorObj = findObjs({
                _characterid: charID,
                _type: "attribute",
                name: attr
            })[0];
            armorObj.set({"current":armorStats[armorIndex],"max":armorStats[armorIndex]});
            log(armorObj);
        }
        
        //will go through each entry in the structAttr table and set structure values
        for (attr in structAttr){
            structStats = armorVal[structAttr[attr]];
            //Structure is the first value in all entries with structure
            //Do not need variable, since it's always 0
            structObj = findObjs({
                _characterid: charID,
                _type: "attribute",
                name: attr
            })[0];
            structObj.set({"current":structStats[0],"max":structStats[0]});
            
        }
    }
    
    function getMP(mechData){
        var mechMove = {};
        
        //Grabs Walking MP entries
        walk = mechData.match(/Walking MP: \d+/g);
        walkKey = "Walking MP";
        walkValue = walk[0].match(/\d+/g);
        mechMove[walkKey] = walkValue;

        //Grabs Running MP entries
        run = mechData.match(/Running MP: \d+/g);
        runKey = "Running MP";
        runValue = run[0].match(/\d+/g);
        mechMove[runKey] = runValue;
        
        //Grabs Jumping MP entries
        jump = mechData.match(/Jumping MP: \d+/g);
        jumpKey = "Jumping MP";
        jumpValue = jump[0].match(/\d+/g);
        mechMove[jumpKey] = jumpValue;
        
        return mechMove;
    }
    
    function setMP(charID,moveProfile) {
        mechWalk = findObjs({
            _characterid: charID,
            _type: "attribute",
            name: "mech_walk"
        })[0];
        
        mechRun = findObjs({
            _characterid: charID,
            _type: "attribute",
            name: "mech_run"
        })[0];
        
        mechJump = findObjs({
            _characterid: charID,
            _type: "attribute",
            name: "mech_jump"
        })[0];
        
        mechSprint = findObjs({
            _characterid: charID,
            _type: "attribute",
            name: "mech_sprint"
        })[0];
        
        //Calculates sprinting speed as (Walk+1)*1.5
        sprintSpeed = Math.ceil((parseInt(moveProfile["Walking MP"][0])+1)*1.5);
        
        mechWalk.set("current",moveProfile["Walking MP"][0]);
        mechRun.set("current",moveProfile["Running MP"][0]);
        mechJump.set("current",moveProfile["Jumping MP"][0]);
        mechSprint.set("current",sprintSpeed);
    }
    
    function resetMech(charID,bio) {
        //Divide input into 3 blocks, based on SSW output
        log(bio)
        mechSpec = bio.split("================================================================================");
        var mechData = mechSpec[0].split("<br>");
        name = mechData[0];
        setName(charID,name);
        tonnageLine = mechSpec[0].match(/Mass\:(.*)/);
        tonnage = mechData[1].match(/\d+/);
        setTonnage(charID,tonnage);
        var armor = getArmor(mechSpec[1]);
        setArmorTest(charID,armor);
        var movement = getMP(mechSpec[1]);
        setMP(charID,movement);
    }
    
    return {
        //Only make ResetMech visible, don't want to make individual functions
        //available for general use
      ResetMech: resetMech
    };
})();

on("chat:message", function (msg) {
    if (msg.type == "api" && msg.content.indexOf("!ResetMech") !== -1) {
        var params = msg.content.split(":");
        playerCount = params.length-1;
        var charID = [];
        
        //Only adds the caller's name to list of mechs to reset
        if (params.length == 1){
            playerCount++;
            var currentChar = findObjs({
                _type: "character",
                controlledby : msg.playerid,
            }); 
            charID.push(currentChar[0].id);
        }
        
        //Adds the mechs of all parameters
        else {
            log(params);
            for (var i=1; i<=playerCount;i++){
                var playerChar = findObjs({
                    _type: "character",
                    name : params[i]
                }); 
                charID.push(playerChar[0].id);
                log(charID);
            }
            
        }
        
        var charID_pass = {};
        for (var j=0; j<playerCount; j++){
            charID_pass = charID[j]
            var character = getObj("character", charID_pass);
            character.get("bio", function(bio) {
                BTImportMech.ResetMech(charID_pass,bio);
            });
        }

        sendChat(msg.who,"All done");
    }
});
