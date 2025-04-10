-- Swedz's Villain Incrementer [Version 1.0] for Tabletop Simulator

all_labels = {
	"A", "B", "C", "D", "E",
	"F", "G", "H", "I", "J",
	"K", "L", "M", "N", "O",
	"P", "Q", "R", "S", "T",
	"U", "V", "W", "X", "Y",
	"Z"
}
last_copy = {}

-- Triggered when a player takes an action
function onPlayerAction(player, action, targets)
	if (action == Player.Action.Copy or action == Player.Action.Cut) and #targets == 1 then
		onStartCopy(targets[1], player)
	elseif action == Player.Action.Paste then
		onPaste(targets[1], player)
	end
	return true
end

-- Triggered when the player begins a copy/clone of an object
function onStartCopy(obj, player)
	last_copy[player.color] = getNameAndLabel(obj)
end

-- Triggered when the player pastes an object
function onPaste(obj, player)
	local player_last_copy = last_copy[player.color]
	if player_last_copy ~= nil then
		local next_label = nextLabel(player_last_copy["label"])
		obj.setName(player_last_copy["name"] .. " " .. next_label)
		player_last_copy["label"] = next_label
	end
end

-- Get the name and label for a given object
-- Grabs the "Zombie" from "Zombie A" as the "name" and the "A" from "Zombie A" as the "label"
function getNameAndLabel(obj)
	local obj_name = obj.getName()
	local _, _, name = string.find(obj_name, "^(.+) %u+$")
	local _, _, label = string.find(obj_name, "^.+ (%u+)$")
	if name == nil or label == nil then
		return nil
	end
	return {
		name = name,
		label = label
	}
end

-- Generate the next label given the current label
-- Order is like: A, B, C, ... Z, AA, AB, ... etc.
function nextLabel(current_label)
	local current_label_start = string.sub(current_label, 1, #current_label - 1)
	local current_label_tail = string.sub(current_label, -1)
	for index = 1, #all_labels do
		local label = all_labels[index]
		if current_label_tail == label and index < #all_labels then
			return current_label_start .. all_labels[index + 1]
		end
	end
	return string.rep(all_labels[1], #current_label + 1)
end